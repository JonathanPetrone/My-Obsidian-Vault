see boot.dev HTTP client course for more!
### JSON Marshaling:

Marshaling is the process of converting a Go struct (or other data types) into a JSON-formatted byte slice. 

```
func createUser(url, apiKey string, data User) (User, error) {
jsonData, err := json.Marshal(data)
...
```

### JSON Decoding:

Decoding typically refers to converting data from an encoded format (e.g., JSON, XML, or Gob) directly from an `io.Reader` (like a file or HTTP response body) into a Go variable. It happens on-the-fly as the data is read.

```
var user User 
decoder := json.NewDecoder(res.Body)
err = decoder.Decode(&user) 
if err != nil { 
	return data, err 
}
```

```decoder := json.NewDecoder(res.Body)```
- **What does it do?**
    - This initializes a JSON decoder to read and parse the JSON response body (`res.Body`). The decoder processes the stream directly from the HTTP response, which avoids loading the entire response into memory.

### Key Differences Decoding vs. Unmarshaling

| Feature           | Decoding                          | Unmarshaling                      |
| ----------------- | --------------------------------- | --------------------------------- |
| **Input**         | `io.Reader` (e.g., streams)       | `[]byte` or `string`              |
| **Memory Usage**  | Stream-based, uses less memory    | Entire JSON is loaded into memory |
| **Use Case**      | Ideal for large or streaming data | Ideal for smaller, in-memory data |
| **Function Used** | `json.NewDecoder` and `Decode()`  | `json.Unmarshal`                  |

### Create a client

The client is responsible for sending HTTP requests (`req`) and receiving responses (`res`). It manages:

- Network communication (e.g., opening sockets).
- Handling SSL/TLS for secure connections.
- Following redirects automatically (if configured).

`&http.Client{}` creates a new HTTP client in Go. It's not entirely "empty" because it has default configurations like transport settings and timeout handling.

**Why do we use it?**

- To perform HTTP operations (e.g., `GET`, `POST`).
- To allow configuration for advanced behaviors, such as setting timeouts, using custom transports, or applying middlewares.

```
client := &http.Client{} 
res, err := client.Do(req)
if err != nil { 
	return data, err 
} 
defer res.Body.Close()
```

### Create a http-request
To do something we need to send a request see http.NewRequest (also see *req* in earlier example). 

The `bytes.NewBuffer(jsonData)` creates a buffer (a wrapper around the byte slice) to serve as the request body. HTTP requests often require a `io.Reader` for the body, and `*bytes.Buffer` satisfies this interface. It allows the data to be read sequentially as the request is processed.

**Is it needed for different HTTP methods?**
For methods like `POST`, `PUT`, or `PATCH` (where you send data), you **need** to pass the body. A buffer is a common way to do this.

For `GET` or `DELETE`, the body is usually empty, so the buffer is unnecessary. Passing `nil` as the body for these methods is typical.

```
req, err := http.NewRequest("POST", url, bytes.NewBuffer(jsonData)) 
if err != nil { 
	return data, err 
}

req.Header.Set("Content-Type", "application/json") req.Header.Set("X-API-Key", apiKey)
```

#### About headers:
**Set Headers**
`Content-Type`: Indicates the data format (JSON).
`X-API-Key`: Provides authentication information.

**Why Do This?**
These steps prepare the request to send properly formatted and authenticated data to the server.