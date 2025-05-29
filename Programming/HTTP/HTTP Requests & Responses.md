#### Requests
At the heart of HTTP is the `HTTP-message`: the format that the text in an HTTP request or response must use. From [RFC 9112 Section 2.1](https://datatracker.ietf.org/doc/html/rfc9112#name-message-format):

```
start-line CRLF
*( field-line CRLF )
CRLF
[ message-body ]
```

![[Skärmavbild 2025-04-06 kl. 20.08.04.png||400]]

This is what a _raw_ HTTP message looks like - specifically a raw HTTP _`GET` request_.
- Request line: `GET /goodies HTTP/1.1`
- Headers: `Host: localhost:42069`, `User-Agent: curl/8.6.0`, `Accept: */*`
- Body: (empty)

### Request line
```http
HTTP-version  = HTTP-name "/" DIGIT "." DIGIT
HTTP-name     = %s"HTTP"
request-line  = method SP request-target SP HTTP-version
```

An example request-line looks like this:
GET /coffee HTTP/1.1 -> (METHOD) - (TARGET) - (HTTPVersion)
### Headers
- **Metadata about the Request**: Headers contain details about the request, such as the type of content being sent, what kind of response is expected, and the client's capabilities.
- **Control the Flow of Data**: Some headers allow the client to control how the server processes the request. For example, the `Accept` header tells the server what type of content the client can handle (e.g., `Accept: application/json`), while the `Content-Length` header indicates the size of the request body.
- **Provide Session or Authentication Information**: Headers such as `Authorization`, `Cookie`, or `Set-Cookie` are used to manage sessions and ensure secure access to resources by including user credentials or session tokens.
- **Facilitate Caching and Efficiency**: Headers like `Cache-Control`, `If-None-Match`, or `ETag` help manage caching behaviors, reducing the need to resend data if it has already been fetched previously.
- **Determine the Communication Type**: Headers like `Connection` or `Transfer-Encoding` influence how data is transferred, specifying whether the connection should be kept alive (`Connection: keep-alive`) or if the data should be transferred in chunks (`Transfer-Encoding: chunked`).

##### Commonly Used and Required HTTP Request Headers:
1. **Host**
    - **Required** in all HTTP/1.1 requests.
    - Specifies the domain name of the server (e.g., `Host: www.example.com`), allowing multiple websites to share the same IP address.
2. **User-Agent**:
    - Identifies the client software making the request (e.g., browser, mobile app). Helps servers tailor responses based on the client type (e.g., `User-Agent: Mozilla/5.0`).
3. **Accept**:
    - Specifies the media types that the client is willing to receive. For example, `Accept: text/html` indicates the client wants HTML content.
4. **Content-Type**:
    - Indicates the type of the data being sent in the body of the request, such as `application/json`, `application/x-www-form-urlencoded`, or `multipart/form-data`.
5. **Content-Length**:
    - **Required** when there is a body in the request. It specifies the size of the request body in bytes.
6. **Authorization** (when needed):
    - Carries credentials for authenticating the client, such as `Authorization: Bearer <token>` for token-based authentication.
7. **Cookie** (if relevant):
    - Contains stored cookies sent by the client, such as session data. Example: `Cookie: session_id=12345;`.
8. **Connection**:
    - Specifies control options for the connection, like whether the connection should be kept open or closed after the request/response cycle (e.g., `Connection: keep-alive`).
9. **Accept-Encoding**:
    - Informs the server about the types of content encodings (e.g., gzip, deflate) the client can handle for compressed responses (e.g., `Accept-Encoding: gzip, deflate`).
10. **If-None-Match** / **If-Modified-Since**:
	- Used for conditional requests. If the resource hasn't been modified since the last request, the server can return a 304 Not Modified status, saving bandwidth.
### Optional but Helpful Headers:
- **Referer**: Provides the address of the previous web page from which the request was made (e.g., for analytics or tracking purposes).
- **X-Requested-With**: Often used in AJAX requests to let the server know the request was made by JavaScript (e.g., `X-Requested-With: XMLHttpRequest`).
- **Cache-Control**: Controls the caching behavior of the request and response, influencing how long resources can be cached (e.g., `Cache-Control: no-cache`).
### Body
The **body** of an HTTP request or response is the part that contains the actual data being transferred between the client and the server. This is where the content of the request or response resides, such as HTML content, JSON data, files, or any other payload. Unlike headers, which only provide metadata about the request or response, the body carries the main content of the exchange.

#### Common Types of Data in Request Bodies:

1. **Form Data**:
    - For submitting forms, data can be encoded as `application/x-www-form-urlencoded` or `multipart/form-data`. The latter is used when uploading files (e.g., in file input fields).
    - Example: `username=johndoe&password=secret`.
2. **JSON**:
    - For API requests, JSON is a common format to send data. This is indicated by the `Content-Type: application/json` header.
    - Example:
        json
        `{   "name": "John Doe",   "age": 30 }`
3. **XML**:
    - Some APIs or services use XML to send data. It’s often indicated with `Content-Type: application/xml`.
    - Example:
        xml
        `<user>   <name>John Doe</name>   <age>30</age> </user>`
4. **Binary Data**:
    - When sending files (images, videos, etc.), the body can contain raw binary data.
    - Example: An image file being uploaded via `Content-Type: image/png`
5. **Text**:
    - Sometimes, plain text data is sent in the body with a `Content-Type: text/plain`.
    - Example: A simple message in the body of a request.
#### Important Characteristics:
- **Size**:
    - The size of the body is often specified by the `Content-Length` header (or `Transfer-Encoding: chunked` for streaming).
- **Encoding**:
    - The body’s content is typically encoded in a format specified by the `Content-Type` header.
- **Semantics**:
    - The body can be used to represent the "resource" being sent, such as user data in a registration form, a file being uploaded, or the content being saved or updated on the server.

---
### Responses:

##### Headers in Responses
While there isn't an official list of all the headers that should be in most responses, there are a few that are common enough that we should include them. Namely:

- [`Content-Length`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Length) (The size of the response body)
- [`Connection`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Connection) (Whether the connection should be kept alive or closed)
- [`Content-Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) (The [MIME](https://developer.mozilla.org/en-US/docs/Web/HTTP/MIME_types/Common_types) type of the response body)

--- Also these
- [`Content-Encoding`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding): Is the response content encoded/compressed? If so, then this should be included to tell the client how to decode it. (Remember, encoded != encrypted)
- [`Date`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Date): The date and time that the message was sent. This is useful for caching and other things.
- [`Cache-Control`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control): Directives for caching mechanisms in both requests and responses. This is useful for telling the client or any intermediaries how to cache the response.

#### Response body 
- The body of an HTTP response contains the data that the server sends back to the client after processing the request.
- It can include content like web pages, images, JSON data, or files, depending on the request and the server's response.

#### Common Types of Data in Response Bodies:
1. **HTML**:
    - The body often contains the HTML markup of a web page that the client will render.
    - Example: A web page’s source code returned with `Content-Type: text/html`.
2. **JSON**:
    - Many APIs return JSON data, which is commonly used for structured data in applications.
    - Example: A response from a weather API might return weather information in JSON format.
3. **XML**:
    - Some web services return XML data. This is common for older web services and some enterprise applications.
    - Example: An API returning weather data in XML format.
4. **Images, Videos, and Files**
    - The body can include binary data for images, videos, and other files. These are returned with appropriate `Content-Type` values (e.g., `Content-Type: image/png` for images).
    - Example: A file download response that sends a PDF file.
5. **Text**:
    - A server might return a plain text response with `Content-Type: text/plain`.
    - Example: A plain-text response from an API or a simple message from a server.
#### Important Characteristics:
- **Size**:    
    - Like request bodies, response bodies are often measured in size and are specified in the `Content-Length` header. For large responses, `Transfer-Encoding: chunked` may be used to send the data in chunks.
- **Encoding**:
    - The `Content-Encoding` header (such as `gzip` or `deflate`) may indicate that the body has been compressed.
- **Semantics**:
    - The body represents the actual resource being returned by the server. For example, if a client requests an image, the body of the response will contain the raw image data.