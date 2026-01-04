## Core Mechanics

For HTMX to make a request:
1. Element must have an `hx-get|post|put|patch|delete` attribute (the HTTP method + URL)
2. A trigger event must fire
    - Either the element's default trigger (click/change/submit)
    - ...or explicitly specified via `hx-trigger`
3. That's it
### Request Flow:

#### 1. Trigger event fires on element
**HTMX has default triggers based on element type.** You only need `hx-trigger` to override the default or add modifiers.

#### 2. Attribute values gathered (inputs processed, headers added)
**Input values are gathered IF:**
The triggering element is inside a `<form>`, OR has `hx-include`, OR has `hx-vals

````html
<!-- Case 1: Trigger inside form - gathers ALL form inputs -->
<form>
  <input name="email" value="test@example.com">
  <input name="age" value="32">
  <button hx-post="/submit">Send</button>
</form>
<!-- Sends: email=test@example.com&age=32 -->

<!-- Case 2: Trigger IS a form - gathers its inputs -->
<form hx-post="/submit">
  <input name="email" value="test@example.com">
  <button type="submit">Send</button>
</form>
<!-- Sends: email=test@example.com -->

<!-- Case 3: Trigger IS an input - gathers closest form -->
<form>
  <input name="email" hx-post="/validate" hx-trigger="blur">
</form>
<!-- Sends: email=<current value> -->

<!-- Case 4: No form, but hx-include specified -->
<button hx-post="/update" hx-include="[name='user_id']">
  Update
</button>
<input type="hidden" name="user_id" value="123">
<!-- Sends: user_id=123 -->

<!-- Case 5: No form, but hx-vals specified -->
<button hx-post="/log" hx-vals='{"action": "click"}'>
  Log
</button>
<!-- Sends: {"action": "click"} -->

<!-- Case 6: No form, no hx-include, no hx-vals -->
<button hx-get="/data">Load</button>
<!-- Sends: NOTHING (empty request body) -->
````

##### Step 2a: Find Values

```
1. Start at triggering element
2. If element is <input>/<select>/<textarea>, include its value
3. Walk up DOM to find closest <form> ancestor
4. If form found, serialize ALL its inputs
5. Process hx-include selector (adds more inputs)
6. Merge in hx-vals JSON
7. Apply hx-params filter (include/exclude specific params)
````

##### Step 2b: Build Headers (ALWAYS happens)

````javascript
// These are ALWAYS added to every HTMX request:
{
  'HX-Request': 'true',
  'HX-Current-URL': window.location.href
}

// These are added IF the data exists:
{
  'HX-Trigger': element.id,           // if element has ID
  'HX-Trigger-Name': element.name,    // if element has name attribute
  'HX-Target': targetElement.id,      // if target has ID
  'HX-Prompt': userInput              // if hx-prompt was used
}
````

##### Step 2c: Serialize Payload
```
IF values exist:
  - Default: application/x-www-form-urlencoded
  - If hx-encoding="multipart/form-data": multipart (for files)
  - If hx-ext="json-enc": application/json

IF no values:
  - Request body is empty
  - But headers still sent
````
#### 3. Request sent to server
Once attribute gathering completes, HTMX **always** sends an HTTP request. There's no conditional logic here—if a trigger fired and gathering completed, the request goes out.

The request is always sent once gathering completes. HTMX adds the `HX-Request: true` header to every request, plus `HX-Current-URL`. Additional HTMX headers (HX-Trigger, HX-Target, HX-Prompt, etc.) are conditionally included based on whether the triggering/target elements have IDs/names/prompts. Standard HTTP headers (Content-Type, X-Requested-With) are added based on request type. You can inject truly custom headers via `hx-headers` or JavaScript events.

### Headers Sent IF Data Exists
```http
HX-Current-URL: https://example.com/dashboard
```
- Current browser location
- Sent on **every request** (not conditional)

```http
HX-Trigger: button-id
```

- ID of the element that triggered the request
- Only sent **IF the triggering element has an `id` attribute**

```http
HX-Trigger-Name: save-button
```

- Name attribute of triggering element
- Only sent **IF the triggering element has a `name` attribute**

```http
HX-Target: results-div
```

- ID of the target element
- Only sent **IF the target element has an `id` attribute**

```http
HX-Prompt: user typed this
```

- User input from `hx-prompt` dialog
- Only sent **IF `hx-prompt` was specified on element**

```http
HX-Active-Element: field-id
```

- ID of currently focused element
- Only sent **IF an element has focus AND has an `id`**

```http
HX-Active-Element-Name: email
```

- Name of currently focused element
- Only sent **IF focused element has a `name` attribute**

```http
HX-Active-Element-Value: test@example.com
```

- Value of currently focused element
- Only sent **IF focused element has a value**

## Standard HTTP Headers Added by HTMX

**Content-Type** (if request has body):

```http
Content-Type: application/x-www-form-urlencoded
```

- Default for POST/PUT/PATCH with form data

```http
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary...
```

- When `hx-encoding="multipart/form-data"` (file uploads)

```http
Content-Type: application/json
```

- When using `hx-ext="json-enc"`
#### 4. Response Received and Swap Prepared 

##### What This Step Does

HTMX receives the server's response and **plans** the DOM update without executing it yet. This is the decision-making phase before modification.

##### The Process

**1. Read HTTP Response**
- Status code (200, 404, 500, etc.)
- Response headers (both standard HTTP and HX-* headers)
- Response body (usually HTML fragment)

**2. Check for Server Instructions (Response Headers)**
HTMX looks for special headers that override client-side attributes:

```http
HX-Retarget: #different-element
```

- Overrides `hx-target` attribute
- Changes where response will be inserted

```http
HX-Reswap: beforeend
```

- Overrides `hx-swap` attribute
- Changes how response will be inserted

```http
HX-Trigger: {"showNotification": "Saved!"}
```

- Events to fire after swap completes
- Can be JSON with event details

```http
HX-Redirect: /success
```

- Full page redirect (browser navigation)
- Skips swapping entirely

```http
HX-Refresh: true
```

- Force full page reload
- Skips swapping entirely

````http
HX-Location: /dashboard
```
- Client-side redirect (HTMX request to new URL)
- Can include swap options as JSON
````

**3. Determine Final Target**
```
Priority order:
1. HX-Retarget header (if present)
2. hx-target attribute (if specified)
3. Triggering element itself (default)
```

**4. Determine Final Swap Strategy**
```
Priority order:
1. HX-Reswap header (if present)
2. hx-swap attribute (if specified)
3. innerHTML (default)
````

**5. Parse Response HTML**

```html
<!-- Server sends this: -->
<div class="result">Main content</div>
<div hx-swap-oob="true" id="notification">
  Out of band content
</div>
```

HTMX splits this into:

- **Primary swap**: `<div class="result">Main content</div>`
- **OOB swaps**: List of elements to swap elsewhere

**6. Extract Fragment (if hx-select used)**

```html
<!-- Element specified: hx-select="#data" -->
<!-- Server response: -->
<html>
  <body>
    <div id="header">...</div>
    <div id="data">This part only</div>
    <div id="footer">...</div>
  </body>
</html>
```

HTMX extracts only: `<div id="data">This part only</div>`

#### 5. DOM updated, events fired

HTMX executes the swap plan from step 4, physically modifying the DOM, then fires events to notify JavaScript that changes happened.

## The Process (Sequential Phases)

### Phase A: Pre-Swap

**1. Fire `htmx:beforeSwap` event**

- Last chance to cancel or modify the swap
- Can change target, swap strategy, or abort entirely
- Event detail contains response data

javascript

```javascript
document.body.addEventListener('htmx:beforeSwap', (e) => {
  if (e.detail.xhr.status === 404) {
    e.detail.shouldSwap = false; // Cancel swap
  }
});
```

**2. Add `.htmx-swapping` class**

- Applied to element being replaced
- Triggers CSS transitions for "old content leaving"

css

```css
.htmx-swapping {
  opacity: 0;
  transition: opacity 200ms;
}
```

**3. Wait for swap delay (if specified)**

html

```html
<div hx-swap="innerHTML swap:500ms">
```

- Delays swap by 500ms
- Allows CSS transitions to complete before DOM changes

### Phase B: Execute Swap

**4. Modify the DOM**

Based on swap strategy:

html

```html
<!-- Target before swap -->
<div id="target">
  <p>Old content</p>
</div>
```

**innerHTML** (default):

javascript

```javascript
target.innerHTML = newContent;
```

html

```html
<div id="target">
  <p>New content</p>
</div>
```

**outerHTML**:

javascript

```javascript
target.outerHTML = newContent;
```

html

```html
<p>New content</p>
<!-- #target is gone, replaced entirely -->
```

**beforebegin**:

javascript

```javascript
target.insertAdjacentHTML('beforebegin', newContent);
```

html

```html
<p>New content</p>
<div id="target">
  <p>Old content</p>
</div>
```

**afterbegin**:

javascript

```javascript
target.insertAdjacentHTML('afterbegin', newContent);
```

html

```html
<div id="target">
  <p>New content</p>
  <p>Old content</p>
</div>
```

**beforeend**:

javascript

```javascript
target.insertAdjacentHTML('beforeend', newContent);
```

html

```html
<div id="target">
  <p>Old content</p>
  <p>New content</p>
</div>
```

**afterend**:

javascript

```javascript
target.insertAdjacentHTML('afterend', newContent);
```

html

```html
<div id="target">
  <p>Old content</p>
</div>
<p>New content</p>
```

**delete**:

javascript

```javascript
target.remove();
```

html

```html
<!-- #target is gone, response ignored -->
```

**none**:

javascript

```javascript
// Nothing happens to DOM
// Used when you only want side effects (events, headers)
```

**5. Process Out-of-Band Swaps**

Each OOB element swaps into its target using its specified strategy:

html

```html
<!-- Response included: -->
<div hx-swap-oob="true" id="counter">5</div>
<div hx-swap-oob="beforeend:#alerts">
  <p class="alert">New alert</p>
</div>
```

HTMX finds `#counter`, replaces its content with "5"  
HTMX finds `#alerts`, appends the alert paragraph

**6. Add Classes to New Content**

- `.htmx-added` - Marks newly inserted elements
- `.htmx-settling` - Indicates settling phase active

### Phase C: Settling

**7. Fire `htmx:afterSwap` event**

- DOM has changed, but settling hasn't finished
- Good for initializing plugins on new content

javascript

```javascript
document.body.addEventListener('htmx:afterSwap', (e) => {
  // Initialize tooltips on new elements
  initTooltips(e.detail.target);
});
```

**8. Initialize New HTMX Elements**

- Scans new content for `hx-*` attributes
- Attaches event listeners to new elements
- New buttons/forms now respond to HTMX triggers

**9. Attribute Inheritance (if applicable)**

- New elements can inherit parent's `hx-*` attributes
- Useful for list items inheriting delete behavior

**10. Wait for Settle Delay**

html

```html
<div hx-swap="innerHTML settle:100ms">
```

- Delays 100ms before removing settling classes
- Allows CSS transitions to run

**11. Remove Settling Classes**

- `.htmx-settling` removed
- `.htmx-added` removed
- CSS can transition elements to final state

css

```css
.htmx-settling .new-item {
  opacity: 0;
  transform: translateY(-10px);
}
.new-item {
  opacity: 1;
  transform: translateY(0);
  transition: all 200ms;
}
```

### Phase D: Finalization

**12. Fire `htmx:load` event**

- Final notification that new content is ready
- Common hook for jQuery plugins, analytics, etc.

javascript

```javascript
document.body.addEventListener('htmx:load', (e) => {
  console.log('New content loaded:', e.detail.elt);
});
```

**13. Update Browser History (if applicable)**

html

```html
<button hx-get="/page2" hx-push-url="true">
```

- Adds entry to browser history
- Updates address bar to `/page2`
- Back button will work

**14. Scroll/Focus Management**

html

```html
<div hx-swap="innerHTML show:top">
```

- Scrolls target to top of viewport
- Or focuses specific element if configured

**15. Fire Server-Triggered Events**

http

```http
HX-Trigger: {"showNotification": "Saved!"}
```

HTMX fires custom event:

javascript

```javascript
document.body.addEventListener('showNotification', (e) => {
  alert(e.detail.value); // "Saved!"
});
```

**16. Cleanup**

- Remove `.htmx-request` from triggering element
- Re-enable elements disabled via `hx-disabled-elt`
- Remove loading indicators

## Timeline Example

html

````html
<button hx-get="/data" 
        hx-target="#result"
        hx-swap="innerHTML swap:200ms settle:100ms">
  Load
</button>
<div id="result">Old</div>
```

**Execution:**
```
T+0ms:    htmx:beforeSwap fires
          Can cancel here if needed
          
T+0ms:    #result gets .htmx-swapping class
          CSS transition starts (opacity fade out)

T+200ms:  Swap delay complete
          DOM modified: "Old" → "New"
          .htmx-swapping removed
          New content gets .htmx-settling, .htmx-added
          
T+200ms:  htmx:afterSwap fires
          Initialize plugins on new content
          
T+200ms:  New HTMX elements initialized
          New content scanned for hx-* attributes

T+300ms:  Settle delay complete
          .htmx-settling, .htmx-added removed
          CSS transition completes (opacity fade in)
          
T+300ms:  htmx:load fires
          Final "content ready" notification
          
T+300ms:  HX-Trigger events fire (if any)
          Custom application logic runs
          
T+300ms:  .htmx-request removed from button
          Button re-enabled, loading indicators hidden
````

## Events Summary

**Fired in order:**

1. `htmx:beforeSwap` - Before DOM changes (can cancel)
2. `htmx:afterSwap` - After DOM modified, before settling
3. `htmx:load` - After settling complete
4. Custom events from `HX-Trigger` header

**Other lifecycle events (not in this step):**

- `htmx:beforeRequest` - Step 2 (before send)
- `htmx:afterRequest` - Step 4 (response received)
