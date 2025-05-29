### HTTP was/is built on TCP and now UDP with http3

**Transmission Control Protocol ([TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol))** is a primary communication protocol of the internet, though that is changing with [HTTP3](https://www.cloudflare.com/learning/performance/what-is-http3/) (which is _not_ built on TCP) gaining adoption.

TCP is great because it allows _ordered data_ to be safely sent across the internet.

When data is sent over a network, it is sent in [packets](https://en.wikipedia.org/wiki/Packet_switching). Each message is split into packets, the packets are sent, they arrive (potentially) out of order, and they are reassembled on the other side. And without a protocol like TCP, you can't guarantee that the order is correct...

**User Datagram Protocol ([UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol))** is often compared to TCP, as they are both [transport layer protocols](https://en.wikipedia.org/wiki/Transport_layer). Here are the high-level differences between the two:

![[Skärmavbild 2025-04-06 kl. 20.02.13.png||300]]

TCP establishes a connection between sender and receiver with a [handshake](https://en.wikipedia.org/wiki/Handshake_\(computing\)), and ensures that all the data is sent in order. UDP yeets the data to the receiver and hopes they can make sense of it.

**Files and network connections behave very similarly**. From the perspective of your code, files and network connections are both just **streams of bytes** that you can read from and write to.

#### Pull vs. Push
When you read from a **file**, you're in control of the reading process. You decide:

- When to read
- How much to read
- When to stop reading.

You _pull_ data from the file.

When you read from a **network connection**, the data is _pushed_ to you by the remote server. You don't have control over when the data arrives, how much arrives, or when it stops arriving. Your code has to be ready to receive it when it comes.

#### Parsing a stream
Unfortunately parsing code tends to be just one edge case after another. Remember how I said TCP **guarantees data to be in order**? That's true, but I never said it had to be _complete_. TCP (and by extension, HTTP) is a _streaming protocol_, which means we receive data in chunks and should be able to parse it as it comes in.

When we _read_ data, all we're doing is moving the data from the reader (which in the case of HTTP is a network connection, but it could be a file as well, our code is agnostic) into our program. When we _parse_, we're taking that data and _interpreting_ it (moving it from a `[]byte` to a `RequestLine` struct). Once its parsed, we can _discard_ it from the buffer to save memory.

#### Binary data
As we've covered, HTTP is a text-based protocol, but **that doesn't mean it can't send binary data**. In fact, HTTP is quite good at sending binary data, and it does so by using the `Content-Type` header to indicate the type of data being sent. For example, if you're sending an image, you might use `Content-Type: image/png`, and if you're sending a video, you might use `Content-Type: video/mp4`.

That way, the client knows how to interpret the body. Rather than reading it as raw text, it can expect a specific format of video data, for example.