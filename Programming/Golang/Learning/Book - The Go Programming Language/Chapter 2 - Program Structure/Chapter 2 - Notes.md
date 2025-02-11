In Go, as in any other programming language, one builds large programs from a small set of basic constructs. `Variables` store values. Simple expressions are combined into larger ones with `operations` like addition and subtraction. Basic types are collected into aggregates like `arrays` and `structs`. `Expressions` are used in `statements` whose execution order is determined by control-flow statements like `if` and `for`. `Statements` are grouped into `functions` for isolation and reuse. `Functions` are gathered into source files and `packages`.

## 2.1 Names
A name always begins with a letter and Case matters. If the name begins with upper-case it is exported across package boundaries.

| Keywords in Go: |
| - | - | - | - | - |
| break | case | chan | const | continue |
| default | defer | else | fallthrough | for |
| func | go | goto | if | import |
| interface | map | package | range | return |
| select | struct | switch | type | var |

| Constants in Go: |
| - | - | - | - |
| true | false | iota | nil | 

| Types in Go: |
| - | - | - | - | - | - |
| int | int8 | int16 | int32 | int64 |  |
| uint | uint8 | uint16 | uint32 | uint64 | uintptr |
| float32 | float64 | complex128 | complex64 |  |  |
| bool | byte | rune | string | error |  |

| Functions in Go: |
| - | - | - | - | - | - | - | - |
| make | len | cap | new | append | copy | close | delete |
| complex | real | imag | 
| panic | recover |

## 2.2 Declarations

A declaration names a program entity and specifies some or all of its properties. There are four major kinds of declarations: `var`, `const`, `type` and `func`.

ch2 - boiling
```
// Boiling prints the boiling point of water.

package main

import "fmt"

const boilingF = 212.0

func main() {
	var f = boilingF
	var c = (f - 32) * 5 / 9
	fmt.Printf("boiling point = %g°F or %g°C\n", f, c)
}
```

ch2 - ftoc
```
//Ftoc prints two Fahrenheit-to-Celsius conversion.

package main

import "fmt"

func main() {
	const freezingF, boilingF = 32.0, 212.0
	fmt.Printf("%g°F = %g°C\n", freezingF, fToC(freezingF)) // "32°F = 0°C"
	fmt.Printf("%g°F = %g°C\n", boilingF, fToC(boilingF)) // "212°F = 100°C"
}

func fToC(f float64) float64 {
	return (f - 32) * 5 / 9
}
```

In this example main calls ftoc twice, with different values.

## 2.3 Variables

A var declaration creates a variable of a particular type, attaches a name to it, and sets its initial value. Each declaration has the general form.

`var name type = expression`

###### Single declaration initialization of variables:
```
var i, j, k int // int, int, int
var b, f, s` = true, 2.3, "four" // bool, float64, string
```

#### 2.3.1 Short Variable Declarations
Within a function, an alternate form called a short variable declaration may be used to declare and initialize local variables. It takes the form:

`name := expression`

###### Single declaration initialization of short variable declarations:
```
i, j := 0, 1
```

###### Important to remember:
- `:=` is a declaration
- `=` is an assignment 

#### 2.3.2 Pointers
A `pointer` value is the address of a `variable`. A `pointer` thus is the location at which a value is stored. Not every value has an address, but every `variable` does. With a `pointer`, we can read or update the value of a `variable` indirectly, without using or even knowing the name of the `variable`, if indeed it has a name.

```
x := 1
p := &x // p, of type *int, points to x
fmt.Println(*p) // "1"
*p = 2 // equivalent to x = 2
fmt.Println(x) // "2"
```

`Pointers` are comparable; two `pointers` are equal if and only if they point to the same variable or both are nil.

```
func incr(p *int) int {
*p++ // increments what p points to; does not change p
return *p
}

v := 1
incr(&v) // side effect: v is now 2
fmt.Println(incr(&v)) // "3" (and v is now 3)
```

###### Pointers and the flag package
`Pointers` are key to the `flag` package, which uses a program's command-line arguments to set the values of certain variables distributed throughout the program.

ch2 - echo4
```
package main

import (
	"flag"
	"fmt"
	"strings"
)

var n = flag.Bool("n", false, "omit trailing newline")
var sep = flag.String("s", " ", "separator")

func main() {
	flag.Parse()
	fmt.Println(strings.Join(flag.Args(), *sep))
	if !*n {
		fmt.Println()
	}
}
```

###### Explanation
- `-n` causes the echo to omit the trailing newline that would normally be printed
- `-s` sep causes it to separate the output arguments by the contents of the sep instead of the default single space.

The variables sep and n are `pointers` to the `flag` variables, which must be accessed indirectly as `*sep` and `*n`.

When the program is run, it must call flag.Parse before the flags are used, to update the flag variables from their default values. The non-flag arguments are available from flag.Args() as a slice of strings.

#### 2.3.3 The new Function
Another way to create a `variable` is top use the built-in function `new`. The expression `new(T)` creates an unnamed variable of type `T`, initializes it to the zero value of `T`, and returns its address, which is a value of type `*T`

```
p := new(int) // p, of type *int, points to an unnamed int variable
fmt.Println(*p) // "0"
*p = 2 // sets the unnamed int to 2
fmt.Println(*p) // "2"
```

New itself is only a syntactic convenience, not a fundamental notion, these two examples have identical behaviors.

Ex. 1
```
func newInt() *int {
	return new(int)
}
```

Ex. 2
```
func newInt() *int {
	var dummy int
	return &dummy
}
```

Each call to new returns a distinct variable with a unique address.

#### 2.3.4 Lifetime of Variables
The lifetime of a `variable` is the interval of time during which it exists as the program executes. The lifetime of a package-level `variable` is the entire execution of the program. By contrast, local `variables` have dynamic lifetimes: a new instance is created each time the `declaration statement` is executed, and the `variable` lives on until it becomes unreachable, at which its storage may be recycled.

###### Heap or Stack
A compiler may choose to allocate local variables on the heap or on the stack.

```
var global *int

func f() {
	var x int
	x = 1
	global = &x
}
```

Here `x` must be heap-allocated because it is still reachable from the `variable` global `f` has returned, despite being declared as a local `variable`; we say `x` escapes from `f`.

```
func g(){
	y := new(int)
	*y = 1
}
```

Since `*y` does not escape from `g`, it is safe for the compiler to allocate `*y` on the stack.

To write efficient programs you need to be aware of the lifetime of variables.

## 2.4 Assignments
The value held by a variable is updated by an assignment statement, which in its simplest form has a variable on the left of the = sign and an expression on the right.

```
x = 1 // named variable
*p = true // indirect variable
person.name // struct field
count[x] = count[x] * scale // array or slice or map element
```

```
v := 1
v++ // same as v = v + 1
v-- // same as v = v - 1
```

#### 2.4.1 Tuple Assignment
Another form of `assignment`, known as `tuple assignment`, allows several `variables` to be assigned at once. All of the right-hand side `expressions` are evaluated before any of the `variables` are updated, making this form the most useful when some of the `variables` appear on both sides of the `assignment`. A case of this happens when we switch values of two `variables`.

```
x, y = y, x
a[i], a[j] = a[j], a[i]
```

###### Function with multiple results
Certain `expressions`, such as a call to a `function` with multiple results, produce several values. When such a call is used in an `assignment statement`, the left-hand side must have as many `variables` as the `function` results.

Often, `functions` use these additional results to indicate som kind of `error`, either by returning an `error` as in the call to `os.Open`, or a `bool`, usually called `ok`.

There are also three operators that sometimes behaves this way too.
```
v, ok = m[key] // map lookup
v, ok = x.(T) // type assertion
v, ok = <-ch // channel receive
```

#### 2.4.2 Assignability
`Assignment statements` are an explicit form of `assignment`, but there are many places in a program where an `assignment` occurs implicitly: a `function` call implicitly assigns the argument values to the corresponding parameter `variables`; a return statement implicitly assigns the `return` operands to the corresponding result `variables`; and a literal expression for a `composite type` such as this `slice`:

```
medals := []string{"gold", "silver", "bronze"}
```

implicitly assigns each element, as if it had been written like this:
```
medals[0] = "gold"
medals[1] = "silver"
medals[2] = "bronze"
```

###### The rule for assignability
- An `assignment`, explicit or implicit, is always legal if the left-hand side (the variable) and the right hand side (the value) have the same type.
- But there are different cases for various types.

## 2.5 Type Declarations
A `type declaration` defines a new named type that has the same underlying type as an existing type. The named type provides a way to separate different and perhaps incompatible uses of the underlying type so that they can't be mixed intentionally.

```
type name underlying-type
```

Type `declarations` most often appear at package-level.

```
// package tempconv performs Celsius and Fahrenheit temperature computations

package tempconv

type Celsius float64

type Fahrenheit float64

const (
	AbsoluteZeroC Celsius = -273.15
	FreezingC Celsius = 0	
	BoilingC Celsius = 100
)

func CToF(c Celsius) Fahrenheit { return Fahrenheit(c*9/5 + 32) }
func FToC(f Fahrenheit) Celsius { return Celsius(f-32) * 5 / 9 }

```

## 2.6 Packages and Files
`Packages` in Go serve the same purposes as libraries or modules in other languages, supporting modularity, encapsulation, separate compilation, and reuse.

Each `package` serves as a separate name space for its declarations. `Packages` also let us hide information by controlling which names are visible outside the `package`, or exported.

#### *Exercise 2.1*
[[Exercise 2.1]]
#### 2.6.1 Imports
Within a Go program, every `package` is identified by a unique string called its `import path`.

In addition to its `import path`, each `package` has a `package` name, which is the short (and not necessarily unique) name that appear in its `package declaration`.

#### Lesson Learned from Newer Go Version

When working with many modules locally you have to define your workspace in go.work. I struggled for hours to get the main.go file in the cf folder to be able to import tempconv.

![[Pasted image 20231127064823.png | 200]]

These commands created the go.work file in the root and did let me use the folders cf and tempconv.
```
jonathanpetrone@MBPsomtJonathan Chapter-2 % go work init
jonathanpetrone@MBPsomtJonathan Chapter-2 % go work use ./cf
jonathanpetrone@MBPsomtJonathan Chapter-2 % go work use ./tempconv
```

The go.work file looks like:
```
go 1.21.3

use (
	./cf
	./tempconv
)
```

I also had two go.mod files, one in each module folder respectively to make it happen.
#### *Exercise 2.2*
[[Exercise 2.2]] 
#### 2.6.2 Package Initialization
Package initialization begins by initializing package-level variables in the order in which they are declared, except that dependencies are resolved first.

```
var a = b + c // a initialized 3rd
var b = f() // b initialized 2nd
var c = 1 // c initialized 1st 
```


#### Bonus:
I researched around what uint means as a part of understanding the exercises. ints are represented in binary and the first number in the binary code is reserved (and it's called the sign) for positive/negative number, uint (unsigned integer) on the other hand lets us use the first binary number. What this means for uint is that we get more available numbers but no negative numbers.

4 bit signed integer example:

**Positive:**
- 0: 0000
- 1: 0001
- 2: 0010
- 3: 0011
- 4: 0100
- 5: 0101
- 6: 0110
- 7: 0111

**Negative:**
- -1: 1111
- -2: 1110
- -3: 1101
- -4: 1100
- -5: 1011
- -6: 1010
- -7: 1001
- -8: 1000

A heuristic to think about this is that positive uses only three 3-binary numbers (4, 2, 1) add these +1 and make it a negative number and you have the value for the negative number (in our case -8), then you just add numbers as per regular in the binary system. 

*A side note:* The binary representation for floats can be found in the IEEE 754.
#### *Exercise 2.3, 2.4, 2.5*
[[Exercise 2.3, 2.4, 2.5]] 

The exercise with extracting signed integers outlines different ways to accomplish this, with the table-lookup method seeming the best.

###### Table-lookup
```
func PopCount(x uint64) int {
	return int(pc[byte(x>>(0*8))] +
		pc[byte(x>>(1*8))] +
		pc[byte(x>>(2*8))] +
		pc[byte(x>>(3*8))] +
		pc[byte(x>>(4*8))] +
		pc[byte(x>>(5*8))] +
		pc[byte(x>>(6*8))] +
		pc[byte(x>>(7*8))])
}
```

This is extracting 8 bits (1 byte) at a time. Ex. 10010101 and then adding the 1s.



## 2.7 Scope
A declaration associates a name with a program entity, such as a function or a variable. The scope of a declaration is the part of the source code where a use of the declared name refers to that declaration.