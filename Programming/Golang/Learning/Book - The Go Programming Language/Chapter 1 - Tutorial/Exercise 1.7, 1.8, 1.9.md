```
package main

import (
	"fmt"
	"io"
	"net/http"
	"os"
	"strings"
)

func main() {

	for _, url := range os.Args[1:] {
	
		hasPrefix := strings.HasPrefix(url, "http://") //1.8
		
		if hasPrefix {
			fmt.Println("The string provided has prefix")
		} else {
			fmt.Println("The string provided does not have the required prefix")
			url = "http://" + url
		}
		
		resp, err := http.Get(url)
		fmt.Println("HTTP STATUS: " + resp.Status) //1.9
		
		if err != nil {
			fmt.Fprintf(os.Stderr, "fetch: %v\n", err)
			os.Exit(1)
		}
		
		b, err := io.Copy(os.Stdout, resp.Body) //1.7
		resp.Body.Close()
		
		if err != nil {
			fmt.Fprintf(os.Stderr, "fetch: reading %s: %v\n", url, err)
			os.Exit(1)
		}
	fmt.Printf("%v", b)
	}
}
```

1.7 change function call from io.ReadAll(resp.Body) to io.Copy(os.Stdout, resp.Body)
1.8 Add prefix "http://" if its missing in string
1.9 Print the status code of the request