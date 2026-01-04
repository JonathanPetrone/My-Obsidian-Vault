### Headers
**Key Headers (Request):**

- `HX-Request: true` - All HTMX requests
- `HX-Trigger` - ID of trigger element
- `HX-Target` - ID of target element
- `HX-Current-URL` - Browser location
- `HX-Prompt` / `HX-Trigger-Name` - User input from prompt/form element name

**Key Headers (Response):**

- `HX-Location` - Client-side redirect (JSON for swap/values)
- `HX-Redirect` - Full page redirect
- `HX-Refresh: true` - Force page refresh
- `HX-Retarget` - Change target selector
- `HX-Reswap` - Override swap strategy
- `HX-Trigger` - Fire events after swap (JSON for details)

## Attribute Quick Reference

### Request Config

html

```html
hx-get|post|put|patch|delete="/url"  <!-- HTTP method + URL -->
hx-trigger="event [modifiers]"        <!-- What fires request -->
hx-target="selector"                  <!-- Where to put response -->
hx-swap="strategy [modifiers]"        <!-- How to insert response -->
hx-include="selector"                 <!-- Additional inputs to send -->
hx-vals='{"key":"value"}'             <!-- Extra JSON values -->
hx-params="param1,param2"             <!-- Filter submitted params -->
```

### Trigger Patterns

````html
hx-trigger="click"                    <!-- Default for buttons -->
hx-trigger="change"                   <!-- Default for inputs -->
hx-trigger="submit"                   <!-- Default for forms -->
hx-trigger="load"                     <!-- On element load -->
hx-trigger="revealed"                 <!-- When scrolled into view -->
hx-trigger="every 2s"                 <!-- Polling -->
hx-trigger="click from:body"          <!-- Event delegation -->
hx-trigger="click consume"            <!-- Prevent default/propagation -->
hx-trigger="click once"               <!-- Fire only once -->
hx-trigger="click changed"            <!-- Only if value changed -->
hx-trigger="click delay:500ms"        <!-- Debounce -->
hx-trigger="click throttle:1s"        <!-- Rate limit -->
hx-trigger="click queue:first"        <!-- Queue handling -->
```

### Swap Strategies
```
innerHTML    - Replace inner content (default)
outerHTML    - Replace entire element
beforebegin  - Before target
afterbegin   - First child of target
beforeend    - Last child of target
afterend     - After target
delete       - Remove target
none         - Don't swap (for side effects)
````

**Swap Modifiers:**

```html
hx-swap="innerHTML swap:500ms"        <!-- Delay swap -->
hx-swap="innerHTML settle:100ms"      <!-- Delay settle (CSS transitions) -->
hx-swap="innerHTML transition:true"   <!-- Enable view transitions -->
hx-swap="innerHTML scroll:top"        <!-- Scroll position after swap -->
hx-swap="innerHTML show:bottom"       <!-- Show element after swap -->
hx-swap="innerHTML focus-scroll:true" <!-- Preserve focus scroll -->
```

### Request Control

```html
hx-push-url="true"                    <!-- Update browser URL -->
hx-push-url="/new-url"                <!-- Set specific URL -->
hx-replace-url="true"                 <!-- Replace URL without history -->
hx-confirm="Are you sure?"            <!-- Confirmation dialog -->
hx-prompt="Enter value"               <!-- User input prompt -->
hx-disabled-elt="selector"            <!-- Disable during request -->
hx-indicator="selector"               <!-- Show loading indicator -->
hx-encoding="multipart/form-data"     <!-- File upload -->
hx-ext="extension-name"               <!-- Load extension -->
hx-boost="true"                       <!-- Progressive enhancement for links -->
```

### Response Handling

```html
hx-select="selector"                  <!-- Extract fragment from response -->
hx-select-oob="selector"              <!-- Out-of-band swap selector -->
hx-sync="selector:drop"               <!-- Abort if request in flight -->
hx-sync="selector:queue"              <!-- Queue requests -->
```

## Out-of-Band Swaps

**In Response HTML:**

```html
<!-- Primary swap target gets normal response -->
<div hx-swap-oob="true" id="alerts">  <!-- Also swaps into #alerts -->
  <p>New alert</p>
</div>
<div hx-swap-oob="outerHTML:#status"> <!-- Custom strategy + target -->
  <span>Updated</span>
</div>
```

**Use for:** Updating multiple page areas from single request (notifications, status bars, counters)

## Events (JavaScript Integration)

**Lifecycle:**

```javascript
// Before request
htmx.on('htmx:configRequest', (e) => {
  e.detail.headers['X-Custom'] = 'value';
});

// After swap
htmx.on('htmx:afterSwap', (e) => {
  // Initialize plugins, run code on new content
});

// Error handling
htmx.on('htmx:responseError', (e) => {
  console.log(e.detail.xhr.status);
});
```

**Common Events:**

- `htmx:beforeRequest` - Modify request before send
- `htmx:afterRequest` - Request completed
- `htmx:beforeSwap` - Control swap behavior
- `htmx:afterSwap` - DOM updated
- `htmx:load` - New content added to DOM
- `htmx:responseError` - HTTP error response

**Programmatic Control:**

```javascript
htmx.trigger('#my-div', 'customEvent');  // Fire event
htmx.ajax('GET', '/url', '#target');     // Manual request
htmx.remove('#element');                 // Remove with cleanup
```

## Extensions

**Enable:**

```html
<div hx-ext="json-enc">  <!-- On container -->
<div hx-get="/url" hx-ext="json-enc">  <!-- On element -->
```

**Common Extensions:**

- `json-enc` - Send JSON instead of form encoding
- `morphdom` - Morph DOM instead of replace (preserves focus/state)
- `alpine-morph` - Use Alpine.js morphing
- `ws` - WebSocket support
- `sse` - Server-sent events

## Form Patterns

**Basic Submission:**

```html
<form hx-post="/submit" hx-target="#result">
  <input name="email" required>
  <button type="submit">Submit</button>
</form>
```

**Inline Validation:**

```html
<input name="email" 
       hx-post="/validate" 
       hx-trigger="blur changed"
       hx-target="next .error">
<span class="error"></span>
```

**Dependent Selects:**

```html
<select name="category" 
        hx-get="/subcategories" 
        hx-target="#subcategory">
  <option>Books</option>
</select>
<select id="subcategory" name="subcategory"></select>
```

## Common Server Response Patterns

**Validation Errors (422):**

```html
<input name="email" class="error">
<span class="error">Invalid email</span>
```

**Success with Redirect (200 + HX-Redirect):**

```go
w.Header().Set("HX-Redirect", "/success")
w.WriteHeader(200)
```

**Trigger Client Events (HX-Trigger):**

```go
w.Header().Set("HX-Trigger", `{"showMessage": "Saved!"}`)
```

**Multi-Area Update (OOB):**

```html
<div id="main-content">Updated content</div>
<div hx-swap-oob="true" id="notification">
  <p>Action completed</p>
</div>
```

## CSS Integration

**Loading States (automatic):**

```css
.htmx-request { opacity: 0.5; }           /* Element making request */
.htmx-request .spinner { display: block; } /* Show spinner during request */
```

**Custom Indicators:**

```html
<button hx-post="/save" hx-indicator="#spinner">Save</button>
<div id="spinner" class="htmx-indicator">Loading...</div>
```

```css
.htmx-indicator { display: none; }
.htmx-request .htmx-indicator { display: block; }
```

**Swap Classes (for transitions):**

```css
.htmx-swapping { opacity: 0; transition: opacity 200ms; }
.htmx-settling { opacity: 1; }
```

## Configuration

**Global Defaults:**

```javascript
htmx.config.defaultSwapStyle = 'outerHTML';
htmx.config.defaultSwapDelay = 0;
htmx.config.defaultSettleDelay = 20;
htmx.config.historyCacheSize = 10;
htmx.config.timeout = 120000;  // 2 minutes
```

**Meta Tags:**

```html
<meta name="htmx-config" 
      content='{"defaultSwapStyle":"outerHTML"}'>
```