
```
package main

import (
"fmt"
"os"
)

func main() {

sep := " "
fmt.Println(os.Args[0])

for i, arg := range os.Args[1:] {
	s := sep + arg
	fmt.Printf(`Index %v:`, i)
	fmt.Println(s)
	}
}
```

1.1 print os.Args[0]
1.2 print index and value once per line for every argument 
1.3 benchmarked the different echo functions echo1 was slowest, exercise was equivalent to echo2 and echo3

*Insights:* printf is needed for using %v, println gives nl, print just writes on the line