### HTML text template
##### Key point: `.` is whatever you pass to ExecutE
```go
{{template "name" .}}
```

```go
// Different handlers can pass different types
tmpl.Execute(w, "just a string")  // '.' is a string
tmpl.Execute(w, 42)               // '.' is an int
tmpl.Execute(w, myStruct)         // '.' is that struct
```

### Navigating Docs
https://pkg.go.dev/text/template#hdr-Actions for text/template examples of keywords

#### Practical heuristic 
**When stuck finding docs:**
1. Is there a comment saying "see package X"? → Go there
2. Is package name `x/y` (subpackage)? → Check parent `x` too
3. Does it implement interface Z? → Read Z's docs for contract
4. Is it a common pattern (Reader, Writer, Handler)? → Read the interface package

#### html/template specifically 
``` text/template (base) 
├─ Actions ← ALL syntax here 
├─ Functions ← Built-in functions 
├─ Pipelines ← Data flow 
└─ Examples ← Usage patterns

html/template (wrapper)
├─ "wraps text/template" ← The key line
├─ Security model ← New content
└─ Escaping contexts ← New content
```

### io
`io` is the universal abstraction layer for "things that move data

Everything that moves bytes implements these interfaces:
```go
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

type Closer interface {
    Close() error
}
```


## The three patterns

go

````go
// 1. SPRINTF family - returns string
s := fmt.Sprintf("Hello %s", name)    // → string
s := fmt.Sprint("Hello", name)        // → string
s := fmt.Sprintln("Hello", name)      // → string

// 2. PRINTF family - writes to stdout (converts to bytes)
fmt.Printf("Hello %s", name)          // → os.Stdout.Write([]byte)
fmt.Print("Hello", name)              // → os.Stdout.Write([]byte)
fmt.Println("Hello", name)            // → os.Stdout.Write([]byte)

// 3. FPRINTF family - writes to any io.Writer (converts to bytes)
fmt.Fprintf(w, "Hello %s", name)      // → w.Write([]byte)
fmt.Fprint(w, "Hello", name)          // → w.Write([]byte)
fmt.Fprintln(w, "Hello", name)        // → w.Write([]byte)
```

## The naming pattern
```
Sprint  = String + Print  → returns string
Printf  = Print + f(ormat) → writes to stdout
Fprintf = File + Print + format → writes to io.Writer (historical name)
````