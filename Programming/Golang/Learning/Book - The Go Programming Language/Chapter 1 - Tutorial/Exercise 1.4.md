```
package main

import (
"bufio"
"fmt"
"os"
"sort"
)

func main() {

counts := make(map[string]map[string]int) // Modified map structure
files := os.Args[1:]

if len(files) == 0 {
	countLines(os.Stdin, counts)
} else {
	for _ , arg := range files {
	f, err := os.Open(arg)
	if err != nil {
		fmt.Fprintf(os.Stderr, "dup2: %v\n", err)
	}
	countLines(f, counts)
	f.Close()
	}
}

for line, fileCounts := range counts {
	totalCount := 0
	var filenames []string

	for filename, count := range fileCounts {
		totalCount += count
		filenames = append(filenames, filename)
	}
	if totalCount > 1 {
		sort.Strings(filenames) // Sort filenames for consistent order
		fmt.Printf("%d occurences of %s in: %s \n", totalCount, line, filenames)
		}
	}
}

  

func countLines(f *os.File, counts map[string]map[string]int) {

	input := bufio.NewScanner(f)

	for input.Scan() {
		line := input.Text()
		if _, ok := counts[line]; !ok {
			counts[line] = make(map[string]int)
		}
		counts[line][f.Name()]++
	}
// NOTE: ignoring potential errors from input.Err()
}
```

1.4 change dup2 to show duplicates and in which files they occur.

*Insights:* -

Todo: Better understanding of maps.