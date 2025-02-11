```
package tempconv

import "fmt"
  
type Celsius float64
type Fahrenheit float64
type Kelvin float64

const (
	AbsoluteZeroC Celsius = -273.15
	FreezingC Celsius = 0
	BoilingC Celsius = 100
)

func (c Celsius) String() string { return fmt.Sprintf("%g°C", c) }

func (f Fahrenheit) String() string { return fmt.Sprintf("%g°F", f) }

func (k Kelvin) String() string { return fmt.Sprintf("%g°K", k) }
```

```
package tempconv

func CToF(c Celsius) Fahrenheit { return Fahrenheit(c*9/5 + 32) }

func FToC(f Fahrenheit) Celsius { return Celsius(f-32) * 5 / 9 }

func KToC(k Kelvin) Celsius { return Celsius(k - 273.15) }

func CToK(c Celsius) Kelvin { return Kelvin(c + 273.15) }

func FToK(f Fahrenheit) Kelvin { return Kelvin(f-32)*5/9 + 273.15 }

func KToF(k Kelvin) Fahrenheit { return Fahrenheit(1.8*(k-273.15) + 32) }
```

**Tests:**
```
package tempconv

import (
"fmt"
"testing"
)

func TestOutput(t *testing.T) {

	t.Run("testing conversion C to F", func(t *testing.T) {
		var input Celsius = 100
		actualOutput := CToF(input)
		expectedOutput := Fahrenheit(212)
		fmt.Println(actualOutput, expectedOutput)
	})
	
	t.Run("testing conversion F to C", func(t *testing.T) {
		var input Fahrenheit = 100
		actualOutput := FToC(input)
		expectedOutput := Celsius(37.7778)
		fmt.Println(actualOutput, expectedOutput)
	})
	
	t.Run("testing conversion K to C", func(t *testing.T) {
		var input Kelvin = 100
		actualOutput := KToC(input)
		expectedOutput := Celsius(-173.15)
		fmt.Println(actualOutput, expectedOutput)
	})
	
	t.Run("testing conversion C to K", func(t *testing.T) {
		var input Celsius = 100
		actualOutput := CToK(input)
		expectedOutput := Kelvin(373.15)
		fmt.Println(actualOutput, expectedOutput)
	})
	
	t.Run("testing conversion F to K", func(t *testing.T) {
		var input Fahrenheit = 100
		actualOutput := FToK(input)
		expectedOutput := Kelvin(310.928)
		fmt.Println(actualOutput, expectedOutput)
	})

	t.Run("testing conversion K to F", func(t *testing.T) {
		var input Kelvin = 100
		actualOutput := KToF(input)
		expectedOutput := Fahrenheit(-279.67)
		fmt.Println(actualOutput, expectedOutput)
	})

}
```

#### Exercise 2.1
Added Kelvin as a scale for temperatures. Tests are not using asserts, and isn't part of the exercise but used to gauge progress towards a working program.