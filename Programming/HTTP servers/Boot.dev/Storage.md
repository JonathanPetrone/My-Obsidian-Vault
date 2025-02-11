Arguably the most important part of your typical web application is the storage of data. It would be pretty useless if each time you logged into your account on YouTube, Twitter or GitHub, all of your subscriptions, tweets, or repositories were gone.

## Memory vs. Disk

When you run a program on your computer (like our HTTP server), the program is loaded into _memory_. Memory is a lot like a scratch pad. It's fast, but it's not permanent. If the program terminates or restarts, the data in memory is _lost_.

When you're building a web server, any data you store in memory (in your program's variables) is lost when the server is restarted. Any important data needs to be saved to disk via the file system.

## Option 1: Raw Files

We _could_ take our user's data, serialize it to JSON, and save it to disk in `.json` files (or any other format for that matter). It's simple, and will even work for small applications. Trouble is, it will run into problems _fast_:

- **Concurrency**: If two requests try to write to the same file at the same time, you'll get overwritten data.
- **Scalability**: It's not efficient to read and write large files to disk for every request.
- **Complexity**: You'll have to write a lot of code to manage the files, and the chances of bugs are high.

## Option 2: a Database

At the end of the day, a database technology like MySQL, PostgreSQL, or MongoDB "just" writes files to disk. The difference is that they _also_ come with all the fancy code and algorithms that make managing those files efficient and safe. In the case of a SQL database, the files are abstracted away from us entirely. You just write SQL queries and let the DB handle the rest.

**We will be using option 2: [PostgreSQL](https://www.postgresql.org/).** It's a production-ready, open-source SQL database. It's a great choice for many web applications, and as a back-end engineer, it might be the single most important database to be familiar with.
