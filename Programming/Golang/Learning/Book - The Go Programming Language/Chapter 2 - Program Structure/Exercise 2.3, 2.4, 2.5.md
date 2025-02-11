```
package popcount

// pc[i] is the population count of 1.

var pc [256]byte

  

func init() {
	for i := range pc {
		pc[i] = pc[i/2] + byte(i&1)
	}
}

  

// PopCount returns the population count (number of set bits) of x.

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

func PopCountLoop(x uint64) int {
	count := 0
	for i := uint64(0); i < 8; i++ {
		count += int(pc[byte(x>>(i*8))])
	}
	return count
}

func BitShiftPopCount(x uint64) int {
	count := 0
	for i := uint64(0); i < 64; i++ {
	if x&1 == 1 {
		count++
	}
		x >>= 1
	}
	return count
}

func LSBPopCount(x uint64) int {
	count := 0
	for x != 0 {
		x &= x - 1
		count++
	}
	return count
}
```

2.3:
2.4:
2.5:
Borrowed solution from https://github.com/kdama/gopl/blob/master/ch02/ex05/popcount.go