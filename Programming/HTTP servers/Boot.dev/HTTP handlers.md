### HTTP handlers
A Handler responds to an HTTP request.
```go
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}
```

Any type with a `ServeHTTP` method that matches the [http.HandlerFunc](https://pkg.go.dev/net/http#HandlerFunc) signature above is an `http.Handler`. Take a second to think about it: it makes a lot of sense! To handle an incoming HTTP request, all a function needs is a way to write a response and the request itself.

You'll typically use a `HandlerFunc` when you want to implement a simple handler. The `HandlerFunc` type is just a function that matches the `ServeHTTP` signature above.

```
type HandlerFunc func(ResponseWriter, *Request)
```

### Why This Signature?

The `Request` argument is fairly obvious: it contains all the information about the incoming request, such as the HTTP method, path, headers, and body.

The `ResponseWriter` is less intuitive in my opinion. The response is an _argument_, not a _return type_. Instead of returning a value all at once from the handler function, we _write_ the response to the `ResponseWriter`.

### Stateful Handlers

It's frequently useful to have a way to store and access state in our handlers. For example, we might want to keep track of the number of requests we've received, or we may want to pass around an open connection to a database, or credentials to an API.

type apiConfig struct {
	fileserverHits atomic.Int32
}

In this course I tracked the number of requests by building a middleware. A
middleware is a way to wrap a handler with additional functionality. It is a common pattern in web applications that allows us to write DRY code.

```
func (cfg *apiConfig) middlewareMetricsInc(next http.Handler) http.Handler {
	return http.HandlerFunc(func(rw http.ResponseWriter, r *http.Request) {
		cfg.fileserverHits.Add(1)
		next.ServeHTTP(rw, r)
	})
}
```

That means this executes first if I "wrap" a handler within it, like:

```
mux.Handle("/app/", apiCfg.middlewareMetricsInc(handler))
```
