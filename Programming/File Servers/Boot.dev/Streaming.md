Giant audio files (like audio books), and especially large video files should be _streamed_ rather than _downloaded_. At least if you want your user to be able to start consuming the content immediately.

## Streaming vs. Downloading

- **Downloading** is when you wait for the entire file to be transferred before you can start using it.
- **Streaming** is when you start using the file immediately while it's still being transferred in the background.

The _simplest_ way to stream a video file on the web (imo) is to take advantage of two things:

1. **The native HTML5 `<video>` element**. It streams video files by default as long as the server supports it.
2. **The [`Range` HTTP header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Range)**. It allows the client to request specific byte ranges of a file, enabling partial downloads. S3 servers support it by default.

_Writing streaming from scratch is hard_, but _using the right tools_ makes it pretty easy these days.