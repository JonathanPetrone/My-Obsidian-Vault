Go's types fall into four categories: `basic types` , `aggregate types` , `reference types` and `interface types`.

`Basic types` = Numbers, strings & booleans
`Aggregate types` = Arrays & structs
`Reference types` = Pointers, slices, maps & functions
`Interface types` = Interfaces

## 3.1 Integers
Go provides both signed and unsigned integer arithmetic (addition, subtraction, multiplication, etc.). 
#### Signed Integers:
- Signed integers can represent both **positive** and **negative** numbers.
- They include a **sign bit**, which determines if the number is positive (0) or negative (1).
- Example types in Go: `int`, `int8`, `int16`, `int32`, `int64`.
- For example, an `int8` (8-bit signed integer) can store values in the range of `-128 to 127`.
#### Unsigned Integers:
- Unsigned integers represent only **non-negative** numbers (i.e., positive numbers and zero).
- They don't have a sign bit, so all bits are used for representing the magnitude of the number.
- Example types in Go: `uint`, `uint8`, `uint16`, `uint32`, `uint64`.
- For example, a `uint8` (8-bit unsigned integer) can store values in the range of `0 to 255`.

The type rune is a synonym for int32 and conventionally indicates that a value is a Unicode code point. Similarly, the type byte is a synonym for uint8, and emphasizes that the value is a piece of raw data rather than a small numeric quantity.

- `rune` = `int32` (for Unicode code points)
- `byte` = `uint8` (for raw data)

There is also an unsigned integer type called uintptr used for low-level programming.

Also read about binary operators for arithmetic and this prompted med to ask ChatGPT about >> << operators as they were part of an earlier exercise. They shift the bits of numbers to the left (<<) or right (>>).

```
var a uint8 = 8 
// 8 is 00001000 in binary
result := a >> 2  
// Shift right by 2 bits
fmt.Println(result)
// Output: 2, which is 00000010 in binary
```

If the result of an arithmetic operation, whether signed or unsigned, has more bits than can be represented in the result type, it is said to `overflow`.

## 3.2 Floating-Point Numbers
Go provides two sizes of floating-point numbers, float32 and float64. float64 should be the preferred choice for most purposes, because float32 computations accumulate errors quickly.

#### *Exercise 3.1, 3.2, 3.3, 3.4*
[[Exercise 3.1, 3.2, 3.3, 3.4]]
## 3.3 Complex Numbers
Go provides two sizes of complex numbers, complex64 and complex128, whose components are float32 and float64 respectively.

#### *Exercise 3.5, 3.6, 3.7, 3.8, 3.9*
[[Exercise 3.5, 3.6, 3.7, 3.8, 3.9]] 
## 3.4 Booleans


## 3.5 Strings
#### 3.5.1 String Literals
#### 3.5.2 Unicode
#### 3.5.3 UTF-8
#### 3.5.4 Strings and Byte Slices
#### *Exercise 3.10, 3.11, 3.12*
[[Exercise 3.10, 3.11, 3.12]]
#### 3.5.5 Conversions between Strings and Numbers
## 3.6 Constants
#### 3.6.1 The Constant Generator iota
#### *Exercise 3.13*
[[Exercise 3.13]] 
#### 3.6.2 Untyped Constants
