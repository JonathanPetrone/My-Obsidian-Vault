## 1.1 Tutorial
Go code is organized in `packages`, which are similar to libraries and modules in other languages. A `package` consists of one or more `.go` source files in a single directory that define what a `package` does.

Package main is special. It defines a standalone executable program, not a library. Within `package main` the function `main` is also special-it's where the execution of the program begins.

## 1.2. Command-Line Arguments
The `os` package provides functions and other values for dealing with the operating system in a platform-independent fashion. Command-line arguments are available to a program in a variable name Args that is part of the `os` package; thus its name anywhere outside the `os` package is `os.Args`.

`os.Args[0]` is the function that invoked it, not the first index.

Three code examples were shown in this part of the book of uses of `os.Args`.
#### echo1
```
func main() {

	var s, sep string
	
	for i := 1; i < len(os.Args); i++ {
		s += sep + os.Args[i]
		sep = " "
	}
	
	fmt.Println(s)

}
```

#### echo2 - use of range
`range` produces a pair of values: the index and the value.

Since we don't use the index we can use `_`(blank identifier) since the syntax needs it. 

```
func main() {

	s, sep := "", ""
	
	for _, arg := range os.Args[1:] {
		s += sep + arg
		sep = " "
	}
	fmt.Println(s)
}
```

#### echo3 - use of strings.Join
```
func main() {
	fmt.Println(strings.Join(os.Args[1:], " "))
}
```

I ran the program by -> go run main.go 
and then on the same line in the terminal -> first second third
which were my args

#### *Exercise 1.1, 1.2, 1.3*
[[Exercise 1.1, 1.2, 1.3]]

## 1.3 Finding Duplicate Lines
Programs for file copying, printing, searching, sorting, counting, and the like all have a similar structure: a loop over the input, som computation on each element, and generation of output on the fly at the end.

#### Introduction of maps
A map holds a set of key/value pairs and provides constant-time operations to store, retrieve, or test for an item in the set. The key may be of any type whose values can be compared with `==`, strings being the most common example.

The built-in function make creates an new empty map; but it has other uses too.

The book shows different variants of a program called dup.
#### dup1
```
/* dup1 prints the text of each line that appears more than
once in the standard input, preceded by its count */

package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {

	counts := make(map[string]int)
	input := bufio.NewScanner(os.Stdin)
	
	for input.Scan() {
		counts[input.Text()]++
	}

	// NOTE: ignoring potential errors from input.Err()

	for line, n := range counts {
		if n > 1 {
			fmt.Printf("%d\t%s\n", n, line)
		}
	}
}
```


```
counts[input.Text()]++
```

is equal to:

```
line := input.Text()
counts[line] = counts[line] + 1
```

So we check content of line in a file ex. "123" and make it a key to an int (0), we then increment that int.

##### bufio
`input.Scan()` reads the next line and removes the newline character from the end.

The result can then be retrieved by calling `input.Text()`.

#### dup2 - read from stdin or a list of files
```
/* Dup2 prints the count and text of lines that appear more than once in the input.

It reads from sdtdin or from a list of named files. */
  
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {

	counts := make(map[string]int)
	files := os.Args[1:]
	
	if len(files) == 0 {
		countLines(os.Stdin, counts)
	} else {
		for _, arg := range files {
			f, err := os.Open(arg)
			if err != nil {
				fmt.Fprintf(os.Stderr, "dup2: %v\n", err)
			}
			countLines(f, counts)
			f.Close()
		}
	}
	
	for line, n := range counts {
		if n > 1 {
			fmt.Printf("%d\t%s\n", n, line)
		}
	}
}

func countLines(f *os.File, counts map[string]int) {

	input := bufio.NewScanner(f)
	
	for input.Scan() {
		counts[input.Text()]++
	}
// NOTE: ignoring potential errors from input.Err()
}
```

##### os.Open
The function returns two values the first is an open file `(*os.File)` that is used by subsequent reads by the `Scanner`. But it also has an `error`, which should be `nil`, or else something went wrong.

##### Function ordering doesn't matter (package-level)
Functions and other package-level entities may be declared in any order. (see the `countLines` function)

##### More on maps
A `map` is a reference to the data structure created by `make`. When a `map` is passed to a function, the function retrieves a copy of the reference, so any changes the called function makes to the underlying data structure will be visible through the caller's  `map` reference too.

##### Difference solution methods
`dup1` & `dup2` operate in a "streaming" mode in which input is read and broken into lines as needed, so in principle these programs can handle an arbitrary amount of input.

`dup3` will show another way to solve this problem by reading the entire input all at once, split it into lines all at once and then process them. `dup3` only reads named files.

#### dup3 
```
/* Dup3 prints the count and text of lines

that appear more than once in the named input files */

package main

import (
	"fmt"
	"os"
	"strings"
)

func main() {
	counts := make(map[string]int)
	
	for _, filename := range os.Args[1:] {
	
		data, err := os.ReadFile(filename)
	
		if err != nil {
			fmt.Fprintf(os.Stderr, "dup3: %v\n", err)
			continue
		}
	
		for _, line := range strings.Split(string(data), "\n") {
			counts[line]++
		}
	}
	
	for line, n := range counts {
		if n > 1 {
			fmt.Printf("%d\t%s\n", n, line)
		}
	}
}
```

###### Ioutil
`ioutil` is deprecated so we change that from the book to `os.Readfile`.
###### os.ReadFile
`ReadFile` returns a byte slice that must be converted into a string so it can be split by `strings.Split`.

Under the covers, `bufio.Scanner`, `os.ReadFile` and `os.WriteFile` use the Read and Write methods of `*os.File`, but it's rare that most programmers need to access those lower-level routines directly.
###### string()
Not mentioned in detail in the book at this time but `string(data)` converts the bytes to string and I guess that work with string(1) as well (int).

#### *Exercise 1.4*
[[Exercise 1.4]]

## 1.4 Animated GIFs
In this section of the chapter we explore a usage of the `image` packages in the standard Go library.

The code examples shows the use of `const`, `struct` types and `composite literals`. The point of this part is to show what you can do with the standard library.
```
package main

import (
"image"
"image/color"
"image/gif"
"io"
"math"
"math/rand"
"os"
)

var palette = []color.Color{color.White, color.Black}

const (
	whiteIndex = 0
	blackIndex = 1
)

func main() {
	lissajous(os.Stdout)
}

func lissajous(out io.Writer) {

	const (
		cycles = 5
		res = 0.001
		size = 100
		nframes = 64
		delay = 8
	)

	freq := rand.Float64() * 3.0
	anim := gif.GIF{LoopCount: nframes}
	phase := 0.0

	for i := 0; i < nframes; i++ {
		rect := image.Rect(0, 0, 2*size+1, 2*size+1)
		img := image.NewPaletted(rect, palette)

		for t := 0.0; t < cycles*2*math.Pi; t += res {
			x := math.Sin(t)
			y := math.Sin(t*freq + phase)

			img.SetColorIndex(size+int(x*size+0.5), size+int(y*size+0.5), blackIndex)
		}

	phase += 0.1
	anim.Delay = append(anim.Delay, delay)
	anim.Image = append(anim.Image, img)
	}
	gif.EncodeAll(out, &anim) //NOTE: ignoring encoding errors
}
```

###### Referring to package with multiple components
After importing a package whose path has multiple components, like `image/color`, we refer to the package with a name that comes from the last components. So `color.White` belongs to `image/color`, and `gif.GIF` belongs to `image/gif`.
###### const
A `const` declaration gives names to constants, that is, values that are fixed at compile time, such as the numerical parameters for cycles, frames and delay.
###### The struct type
`gif.GIF` is a `struct` type. A struct is a group of values called fields, often of different types, that are collected together in a single object that can be treated as a unit.

#### *Exercise 1.5, 1.6*
[[Exercise 1.5, 1.6]]

## 1.5 Fetching a URL
Go provides a collection of `packages`, grouped under `net`, that make it easy to send and receive information through the Internet, make low-level network connections, and set up servers.

Example:
```
package main

import (
	"fmt"
	"io"
	"net/http"
	"os"
)

func main() {
	for _ , url := range os.Args[1:] {
		resp, err := http.Get(url)
		if err != nil {
			fmt.Fprintf(os.Stderr, "fetch: %v\n", err)
			os.Exit(1)
		}
		b, err := io.ReadAll(resp.Body)
		resp.Body.Close()
		if err != nil {
			fmt.Fprintf(os.Stderr, "fetch: reading %s: %v\n", url, err)	
			os.Exit(1)
		}
	fmt.Printf("%s", b)
	}
}
```

**note:** the book references `io/ioutil`, which is outdated and as of later versions of Go we use just `io`.

This program fetches the content of a specified URL and prints it as uninterpreted text. (Inspired by curl)

#### *Exercise 1.7, 1.8, 1.9*
[[Exercise 1.7, 1.8, 1.9]]

## 1.6 Fetching URLs Concurrently
This section is about an introduction to concurrent programming with examples of `channels` and `goroutines`.

This coming example will fetch many URLs at the same time.

```
package main

import (
	"fmt"
	"io"
	"net/http"
	"os"
	"time"
)

func main() {

	start := time.Now()
	ch := make(chan string)
	
	for _, url := range os.Args[1:] {
		go fetch(url, ch) // start a goroutine
	}
	
	for range os.Args[1:] {
		fmt.Println(<-ch) // receive from channel ch
	}
	
	fmt.Printf("%.2fs elapsed\n", time.Since(start).Seconds())
}
	
func fetch(url string, ch chan<- string) {
	
	start := time.Now()
	
	resp, err := http.Get(url)
	
	if err != nil {
		ch <- fmt.Sprint(err) // send to channel ch
		return
	}
	
	nbytes, err := io.Copy(io.Discard, resp.Body)
	resp.Body.Close() // dont leak resources
	
	if err != nil {
		ch <- fmt.Sprintf("while reading %s: %v", url, err)
		return
	}
	
	secs := time.Since(start).Seconds()
	ch <- fmt.Sprintf("%.2fs %7d %s", secs, nbytes, url)

}
```

A `goroutine` is a concurrent function execution. A `channel` is a communication mechanism that allows one `goroutine` to pass values of a specified type to another `goroutine`.

The main function creates a channel of strings using `make`. For each command-line argument, the `go` statement in the first `range` loop starts a new `goroutine` that calls fetch asynchronously to fetch the URL using `http.Get`.

When one `goroutine` attempts a send or receive on a `channel`, it blocks until another `goroutine` attempts the corresponding receive or send operation, at which point the value is transferred and both `goroutines` proceed.

Having main do all the printing ensures that output from each `goroutine` is processed as a unit, with no danger of interleaving if two `goroutines` finish at the same time.

#### *Exercise 1.10, 1.11* 
[[Exercise 1.10, 1.11]]

## 1.7 A Web Server

In this section an example of a minimal server that returns the path of the component of the URL used to access the server will be shown.

###### Server1 example:
So: http://localhost:8000/hello -> URL.Path = "/hello" 
```
package main

import (
	"fmt"	
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/", handler) // each request calls handler
	log.Fatal(http.ListenAndServe("localhost:8000", nil))
}

// handler echoes the Path component of the requested URL
func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "URL.Path = %q\n", r.URL.Path)
}
```

![[Pasted image 20231125160012.png]]
###### Server2 example:
This version does the same as above but also counts the number of requests in total.
```
package main

import (
	"fmt"
	"log"
	"net/http"
	"sync"
)

var mu sync.Mutex
var count int

func main() {
	http.HandleFunc("/", handler) // each request calls handler
	http.HandleFunc("/count", counter)
	log.Fatal(http.ListenAndServe("localhost:8000", nil))
}

// handler echoes the Path component of the requested URL
func handler(w http.ResponseWriter, r *http.Request) {
	mu.Lock()
	count++
	mu.Unlock()
	fmt.Fprintf(w, "URL.Path = %q\n", r.URL.Path)
}

func counter(w http.ResponseWriter, r *http.Request) {
	mu.Lock()
	fmt.Fprintf(w, "Count %d\n", count)
	mu.Unlock()
}
```

If two concurrent requests try to update count at the same time, it might not be incremented consistently; the program would have a serious bug called a `race condition`. To avoid this problem, we must ensure that at most one `goroutine` accesses the variable at a time, which is the purpose of the `mu.Lock()` and `mu.Unlock()` calls that bracket each access of count.

###### Server3 example:
```
package main 

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/", handler)
	log.Fatal(http.ListenAndServe("localhost:8000", nil))
}

// handler echoes the HTTP request.

func handler(w http.ResponseWriter, r *http.Request) {

	fmt.Fprintf(w, "%s %s %s\n", r.Method, r.URL, r.Proto)
	
	for k, v := range r.Header {
		fmt.Fprintf(w, "Header[%q] = %q\n", k, v)
	}
	
	fmt.Fprintf(w, "Host = %q\n", r.Host)
	fmt.Fprintf(w, "RemoteAddr = %q\n", r.RemoteAddr)
	
	if err := r.ParseForm(); err != nil {
		log.Print(err)
	}
	
	for k, v := range r.Form {
		fmt.Fprintf(w, "Form[%q] = %q\n", k, v)
	}
}
```

This prints headers and form data associated with the request.
![[Pasted image 20231125171148.png]]
#### *Exercise 1.12*
[[Exercise 1.12]]

## 1.8 Loose Ends
There is a lot more to go than what has already been mentioned, this section talks about a few of these.

###### Control flow
We have covered two fundamental `control-flow statements`, `if` and `for`, but not the `switch` statement.
```
switch() {
case "heads":
	heads++
case "tails":
	tails++
default:
	fmt.Println("landed on the edge")
}
```

We can also use `break` or `continue` to modify the flow of control

###### Named types
A type declaration makes it possible to give a name to an existing type. Since `struct` types are often long, they are nearly always named.

Example:
```
type Point struct {
	x, y int
}

var p Point
```

###### Pointers
Go provides `pointers`, which are values that contain the address of a variable. The `&` operator yields the address of the variable and the `*` operatior retrieves the variable that the pointer refers to.

###### Methods and interfaces
A `method` is a function associated with a named type; Go is unusual in that methods may be attached to almost any named type.

`Interfaces` are abstract types that let us treat different concrete types in the same way based on what methods they have, not how they are represented or implemented.

###### Packages
Go comes with an extensive library of useful `packages`, and the Go community has created and shared many more.

Index of standard library can be found at: https://golang.org/pkg
Community packages can be found at: https://godoc.org

###### Comments
Single-line comments: `//`
Multi-line comments: `/* */`