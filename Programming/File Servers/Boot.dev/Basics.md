Building a (good) web application _almost always_ involves handling "large" files of some kind - whether its static images and videos for a marketing site, or user generated content like profile pictures and video uploads, it always seems to come up.

You're probably already familiar with _small structured_ data; the stuff that's usually stored in a relational database like Postgres or MySQL. I'm talking about simple, primitive data types like:

- `user_id` (integer)
- `is_active` (boolean)
- `email` (string)

**Large files**, or "large assets", on the other hand, are giant blobs of data encoded in a specific [file format](https://en.wikipedia.org/wiki/File_format) and measured in kilo, mega, or gigabytes. As a simple rule:

- If it makes sense to go into an excel spreadsheet, it probably belongs in a traditional database
- If it would normally be stored on your hard drive as its own file, it probably is a "large file"

Large files are interesting because:

1. They're **large in size** (duh) and are thus more performance-sensitive
2. They're usually **accessed frequently**, and this combined with their size can quickly lead to performance bottlenecks

## Sending large files
We don't typically send/upload massive files as single JSON payloads or forms. Instead, we use a different encoding format called [multipart/form-data](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST#multipart_form_submission). In a nutshell, it's a way to send multiple pieces of data in a single request and is commonly used for file uploads. It's the "default" way to send files to a server from an HTML form.

Luckily, the Go standard library's `net/http` package has built-in support for parsing `multipart/form-data` requests. The [http.Request](https://pkg.go.dev/net/http#Request) struct has a method called [ParseMultipartForm](https://pkg.go.dev/net/http#Request.ParseMultipartForm).

It's **usually a bad idea** to store large binary blobs in a database, there are exceptions, but they are rare. So what's the solution? **Store the files on the file system**. File systems are optimized for storing and serving files, and they do it well.

### Mime types
A [mime type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types) is a web-friendly way to describe format of a file. It's kind of like a file extension, but more standardized and built for the web. [[Mime types]]

- Understand what "large" files are and how they differ from "small" structured data
- Build an app that uses [AWS S3](https://aws.amazon.com/s3/) and [Go](https://www.boot.dev/courses/learn-golang) to store and serve assets
- Learn how to manage files on a "normal" (non-s3) filesystem based application
- Learn how to _store and serve_ assets at scale using serverless solutions, like AWS S3
- Learn how to _stream_ video and to keep data usage low and improve performance