The Go Programming Language book written as of 1.5
Currently I run 1.21.3

- Go 1.11 introduced modules.
- Go 1.18 added generics.

**Biggest changes (from ChatGPT):**

1. **Garbage Collector Improvements:** Go's garbage collector has seen various improvements, leading to better performance and reduced pause times.
2. **Context Package:** The context package was introduced to help with cancellation and timeouts in Go programs.
3. **Error Handling:** The introduction of the `errors` package and the `fmt.Errorf` function made error handling more consistent and expressive.
4. **Slices:** Slices saw optimizations, and a new `slicing` syntax was introduced to make slice operations more convenient.
5. **Concurrent Programming:** The language introduced better support for concurrent programming with features like goroutine leak detection and the `sync` package improvements.
6. **Modules:** The introduction of Go Modules in Go 1.11 improved dependency management and versioning.
7. **Generics:** Go introduced support for generics in Go 1.18, which is a significant change that allows you to write more generic and reusable code.
8. **Error Values:** Go 1.13 introduced the `xerrors` package to improve error value handling, including stack trace information.
9. **Time Package:** The `time` package received enhancements, including the `time.ParseDuration` function for parsing time durations.
10. **Web Assembly (Wasm):** Go added experimental support for WebAssembly (Wasm), allowing you to write Go code that can run in web browsers.
11. **Tooling:** The Go toolchain and tools like `go fmt`, `go vet`, and `go mod` have seen improvements and new features.
12. **Standard Library Enhancements:** Many additions and improvements were made to the standard library, providing new functionality and better performance.
13. **Performance Improvements:** Go's runtime and compiler have undergone numerous optimizations for better overall performance.
14. **Compatibility:** Go maintains a strong commitment to backward compatibility, but there have been some breaking changes. Be sure to check the release notes for details on any breaking changes.