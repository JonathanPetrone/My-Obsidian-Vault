
### Global vs. Custom Design Convention
**Go convention in std package:** `nil` often means "use the default/global one"

mux 
```go
mux := http.NewServeMux()
mux.HandleFunc(...)
http.ListenAndServe(":8080", mux)
```

vs. DefaultServeMux
```go
http.HandleFunc(...)  // registers on DefaultServeMux
http.ListenAndServe(":8080", nil)  // uses DefaultServeMux
```

You could mix/match any combination:
- Global mux + custom logger
- Custom mux + global logger
- Custom mux + custom logger
- Global mux + global logger

### Log vs. Fmt
"output for humans" **(fmt)** vs. "logging for developers" **(log).**

Log also can use
log.print
log.fatal ->  writes to output, calls `os.Exit(1)`, program terminates
log.panic -> writes to output, calls `panic()`, stack unwinds (can be recovered)

### html/template
The `{{...}}` syntax is **Go's template language** from the `html/template` package (or `text/template`).

## Key actions available

- **Variables**: `{{.FieldName}}`, `{{$var := .Value}}`
- **Control flow**: `{{if}}`, `{{else}}`, `{{range}}`, `{{with}}`
- **Functions**: `{{len .Items}}`, `{{index .Slice 0}}`
- **Pipelines**: `{{.Title | printf "Title: %s"}}`
- **Comparisons**: `{{if eq .Status "active"}}`, `{{if gt .Count 5}}`
- **Template composition**: `{{template "header" .}}`, `{{define "content"}}`


