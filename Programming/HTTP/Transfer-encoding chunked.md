HTTP/1.1, the **message body** doesn't always need to have a known length in advance. Instead of using the `Content-Length` header, a server can use **chunked transfer encoding** by setting the header:

`Transfer-Encoding: chunked`

With this encoding, the response body is sent as a **series of chunks**, each preceded by its size in **hexadecimal**. The format looks like this:

`<chunk-size in hex>\r\n <chunk data>\r\n ... (repeated for each chunk) 0\r\n \r\n   <-- indicates the end of the message`

This allows data to be streamed in **real-time** or when the total size isn't known beforehand.

**Typical use cases include:**

- Streaming large files
- Real-time updates (e.g. chat or live feed)
- Proxying responses from services that stream data (like `httpbin.org/stream/100`)

By using chunked encoding, a server can start sending parts of the response before it has all the data, which improves performance and responsiveness, especially for long-running or dynamic outputs.

#### Trailers
### 📦 **Chunked Transfer Encoding: The Ending and Trailers**

When using **chunked transfer encoding** in HTTP, the message ends with:

KopieraRedigera

`0\r\n \r\n`

At first, this double CRLF might seem odd—but there's a **reason**: it makes room for something called **trailers**.

---

### 🏷️ **What Are Trailers?**

**Trailers** are like regular headers but sent **after** the message body. They're used to include information that wasn't available before the body was transmitted (like checksums, metadata, etc.).

To use trailers:

1. You declare them in the `Trailer` header upfront.
2. After the final chunk (`0\r\n`), you send the trailer headers.
3. You end with a final `\r\n`.

---

### 🧪 **Example**

```HTTP/1.1 200 OK
Content-Type: text/plain
Transfer-Encoding: chunked
Trailer: Lane, Prime, TJ

1E
I could go for a cup of coffee
B
But not Java
12
Never go full Java
0\r\n
Lane: goober
Prime: chill-guy
TJ: 1-indexer
\r\n
```


---

### ✅ **Why Use Trailers?**

- To include **info calculated during the transfer**, like:
    - Message digests or hashes
    - Logging IDs
    - Signatures or validation info

They're especially useful in streaming or proxy scenarios where the full body isn't known up front.