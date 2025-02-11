## List of types:
- **Basic Types**:
    - **Numbers**:
        - Integers: `int`, `int8`, `int16`, `int32`, `int64`, `uint`, `uint8`, `uint16`, `uint32`, `uint64`
        - Floating-point: `float32`, `float64`
        - Complex numbers: `complex64`, `complex128`
    - **Text**:
        - Strings: `string`
        - Runes (Unicode code points): `rune` (alias for `int32`)
    - **Booleans**: `bool` (`true` or `false`)
- **Composite Types**:
    - **Arrays**: Fixed-size collection of elements of the same type, e.g., `[5]int` (array of 5 integers).
    - **Slices**: Dynamic-size collection of elements, e.g., `[]int`.
    - **Maps**: Key-value pairs, e.g., `map[string]int`.
    - **Structs**: Group of fields, e.g.:
        `type Person struct {     Name string     Age  int }`
- **Reference Types**:
    - **Pointers**: Hold the memory address of a value, e.g., `*int`.
    - **Functions**: Represent executable code, e.g., `func(int) string`.
    - **Channels**: Used for communication between goroutines, e.g., `chan int`.
- **Interface Types**:
    - Define a set of methods a type must implement, e.g
	    type Shape interface { Area() float64 }

### Methods for types
In Go, you can create methods for any **named type** except for interface types. A named type is a type that you define with a name using the `type` keyword. 

This includes:
1. Custom Types Based on Built-in Types
2. Struct Types
3. Alias Types (but not recommended in some cases)