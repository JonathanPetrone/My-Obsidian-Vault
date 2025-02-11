```
package main

import (

	"GoBookExercises/Chapter-2/tempconv"
	"flag"
	"fmt"
	"os"
	"strconv"
	"unitconverter/converters"

)
  

func main() {  
	
	var fc = flag.Bool("fc", false, "convert value from fahrenheit to celsius")
	
	var cf = flag.Bool("cf", false, "convert value from celsius to fahrenheit")
	
	var fm = flag.Bool("fm", false, "convert value from feet to meters")
	
	var mf = flag.Bool("mf", false, "convert value from meters to feet")
	
	var pk = flag.Bool("pk", false, "convert value from pounds to kilograms")
	
	var kp = flag.Bool("kp", false, "convert value from kilograms to pounds")
	
	
	flag.Parse()


	cliArgs := os.Args[1:]
	
	floatv := extractFloat(cliArgs)
	
	
	if *fc {
	
		fmt.Println("-- Fahrenheit to Celsius --")
		
		for _, t := range floatv {
		
			f := tempconv.Fahrenheit(t)
			
			fmt.Printf("Fahrenheit %s to Celsius %s\n", f, tempconv.FToC(f))
		
		}
	
	}
	 
	
	if *cf {
	
		fmt.Println("-- Celsius to Fahrenheit --")
		
		for _, t := range floatv {
		
			c := tempconv.Celsius(t)
			
			fmt.Printf("Celsius %s to Fahrenheit %s\n", c, tempconv.CToF(c))
		
		}
	
	}
	  
	
	if *fm {
	
		fmt.Println("-- Feet to Meters --")
		
		for _, t := range floatv {
		
			f := converters.Feet(t)
			
			fmt.Printf("Feet %s to Meters %s\n", f, converters.FToM(f))
		
		}
	
	}
	  
	
	if *mf {
	
		fmt.Println("-- Meters to Feet --")
		
		for _, t := range floatv {
		
			m := converters.Meters(t)
			
			fmt.Printf("Meters %s to Feet %s\n", m, converters.MToF(m))
		
		}
	
	}
	  
	
	if *pk {
	
		fmt.Println("-- Pounds to Kilograms --")
		
		for _, t := range floatv {
		
			p := converters.Pounds(t)
			
			fmt.Printf("Pounds %s to Kilograms %s\n", p, converters.PToK(p))
		
		}
	
	}
	
	
	if *kp {
	
		fmt.Println("-- Kilograms to Pounds --")
	
		for _, t := range floatv {
		
			k := converters.Kilograms(t)
			
			fmt.Printf("Kilograms %s to Pounds %s\n", k, converters.KToP(k))
		
		}
	}  
	fmt.Println(floatv)
}

  
func extractFloat(cliArgs []string) []float64 {

	numbers := []float64{}
	
	for _, arg := range cliArgs {
	
		f, err := strconv.ParseFloat(arg, 64)
		
		if err == nil {
		
			numbers = append(numbers, f)
		
		}
	}
	return numbers
}
```

#### Exercise 2.2 
The goal was to create a program to handle conversion between different units. I used what I learned from the flag packages and also how the earlier conversion program was structured.

The converter logic is in separate files.

