If you squint _really_ hard, it feels like S3 is a _file_ system in the cloud... but it's not. It's technically an _object_ storage system - which is _not quite_ the same thing.

## Traditional File Storage

"File storage" is what you're already familiar with:

- Files are stored in a hierarchy of directories
- A file's system-level metadata (like timestamp and permissions) is managed by the _file system_, not the file itself

File storage is great for single-machine-use (like your laptop), but it doesn't distribute well across many servers. It's optimized for low-latency access to a small number of files on a single machine.

## Object Storage

Object storage is designed to be more **scalable, available, and durable** than file storage because it can be easily distributed across many machines:

- Objects are stored in a flat namespace (no directories)
- An object's metadata is stored _with_ the object itself

## Organization matters
**Schema architecture matters in a SQL database, and prefix architecture matters in S3**. We always want to group objects in a way that makes sense for our case, because often we'll want to operate on a group of objects at once.

For example, pretend you do the naive thing and upload all your images to the root of your bucket. What happens if...

- you want to delete all the images for a specific user?
- a feature changed and you need to resize all the images it uses?
- you want to change the permissions of all the images associated with a specific organization?

If you don't have any prefixes (directories) to group objects, you might find yourself iterating over every object in the bucket to find the ones you care about. That's _slow_ and _expensive_.