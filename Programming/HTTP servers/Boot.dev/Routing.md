Routing is the process of path selection in any network. The Go standard library has a lot of powerful HTTP features and, as of version 1.22, comes equipped with method-based pattern matching for routing.

Note that there are other powerful routing libraries like [Gorilla Mux](https://github.com/gorilla/mux) and [Chi](https://github.com/go-chi/chi).

## Method Specific Routing
Using the Go standard library, you can specify a method like this: `[METHOD ][HOST]/[PATH]`. For example:

```
mux.HandleFunc("POST /articles", handlerArticlesCreate)
mux.HandleFunc("DELETE /articles", handlerArticlesDelete)
```

### Rules and Definitions

#### Fixed URL Paths
A pattern that exactly matches the URL path. For example, if you have a pattern `/about`, it will match the URL path `/about` and no other paths.
#### Subtree Paths
If a pattern ends with a slash `/`, it matches all URL paths that have the same prefix. For example, a pattern `/images/` matches `/images/`, `/images/logo.png`, and `/images/css/style.css`. As we saw with our `/app/` path, this is useful for serving a directory of static files or for structuring your application into sub-sections.
#### Longest Match Wins
If more than one pattern matches a request path, the longest match is chosen. This allows more specific handlers to override more general ones. For example, if you have patterns `/` (root) and `/images/`, and the request path is `/images/logo.png`, the `/images/` handler will be used because it's the longest match.
#### Host-Specific Patterns
We won't be using this but be aware that patterns can also start with a hostname (e.g., `www.example.com/`). This allows you to serve different content based on the Host header of the request. If both host-specific and non-host-specific patterns match, the host-specific pattern takes precedence.

More details: [ServeMux docs](https://pkg.go.dev/net/http#ServeMux).