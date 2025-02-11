Generics enable you to write more reusable code. Before generics, Go required you to write separate, often duplicated, code for different data types. 

With generics, you can write functions, data structures, and libraries that work with a wide range of data types without sacrificing type safety or performance.

You would use generics in your Go code when you want to write more flexible, reusable, and type-safe functions, data structures, or libraries that work with a variety of data types.

Example:
```package main

import (
	"fmt"
)

// Min finds the minimum value in a slice of any comparable type.

func Min[T comparable](slice []T) T {
	if len(slice) == 0 {
		// Return a zero value for the type T when the slice is empty.
		return T{}
	}
	min := slice[0]
	for _, v := range slice {
		if v < min {
			min = v
		}
	}
	return min
}

func main() {
	intSlice := []int{4, 7, 2, 9, 1}
	floatSlice := []float64{3.14, 1.618, 2.718, 0.577}

	// Find the minimum values in the slices.
	minInt := Min(intSlice)
	minFloat := Min(floatSlice)

	fmt.Printf("Minimum int value: %d\n", minInt)
	fmt.Printf("Minimum float value: %f\n", minFloat)
}
```
