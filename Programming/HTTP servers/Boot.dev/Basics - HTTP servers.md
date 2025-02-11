## What Is a Server?

A web [server](https://en.wikipedia.org/wiki/Server_%28computing%29) is just a computer that serves data over a network, typically the Internet. Servers run software that listens for incoming requests from clients. When a request is received, the server responds with the requested data.

In Go, _goroutines_ are used to serve _many_ requests at the same time.

Servers run forever, waiting for requests to come in, processing them, sending responses, and then waiting for the next request. If they didn't work this way, websites and apps would be down and unavailable _all the time_!

Servers can serve files [[File servers]]

Debugging a server is a little different from a CLI app. The _simplest_ way (minimal tooling) to debug a server is to:

1. Write some code.
2. Build and run the code.
3. Send a request to the server using a browser or some other HTTP client.
4. See if it did what you expected.
5. If it didn't, add some logging or fix the code, and go back to step 2.

Servers are built to be used by clients. As you develop your code, you should be using a tool that makes sending one-off requests to your server easy! Examples:

- [Thunder Client for VS Code](https://marketplace.visualstudio.com/items?itemName=rangav.vscode-thunder-client)
- [REST Client for VS Code](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)
- [Postman for VS Code](https://marketplace.visualstudio.com/items?itemName=Postman.postman-for-vscode)
- [cURL](https://curl.se/)
- [Postman](https://www.postman.com/)

When building HTTP servers in Go, understanding HTTP handlers is important. [[HTTP handlers]]
