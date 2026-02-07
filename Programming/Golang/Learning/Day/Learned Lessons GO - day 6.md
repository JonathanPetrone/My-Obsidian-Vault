In a bare `go func()`, there's **one worker** - that single goroutine.
```go
go func() {
    // This is ONE worker doing work
    fmt.Println("One goroutine")
}()
```

To get multiple workers, you spawn multiple goroutines:
```go
// Three workers
for i := 0; i < 3; i++ {
    go func() {
        // Each of these is a separate worker
    }()
}
```

Go's runtime automatically distributes goroutines across **all available CPU cores** by default.

For CPU-bound work, you typically want workers ≈ number of cores. For I/O-bound work (network, disk), you can have way more workers than cores since they're mostly waiting.

Your worker pool gets automatic parallelism across all cores unless you're on a single-core machine.