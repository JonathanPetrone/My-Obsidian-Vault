There are an infinite number of things we could consider "large files". But within the context of web development, the most common types of large files are probably:

1. **Images**: PNGs, JPEGs, GIFs, SVGs, etc.
2. **Videos**: MP4s, MOVs, AVIs, etc.
3. **Audio**: MP3s, WAVs, etc.
4. **Static web templates**: HTML, CSS, JS, etc.
5. **Administrative files**: PDFs, Word docs, etc.

A [mime type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types) is just a web-friendly way to describe format of a file. It's kind of like a file extension, but more standardized and built for the web.

Mime types have a type and a subtype, separated by a `/`. For example:

- `image/png`
- `video/mp4`
- `audio/mp3`
- `text/html`

When a browser uploads a file via a multipart form, it sends the file's mime type in the [Content-Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) header.