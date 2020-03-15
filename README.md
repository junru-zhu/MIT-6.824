# MIT-6.824

## Lecture 1

## Lecture 2
### Pre-class Assignment: [A Tour of Go](https://tour.golang.org/welcome/1) ###

**Questions**

1. what is the range of numbers represented by float64?<br/>Answer: 

2. what is the significance of the time 2009-11-10 23:00:00 UTC for Go playground?<br/>Answer:

**Notes**

1. Defer: A defer statement defers the execution of a function until the surrounding function returns. Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.

```
func main() {
	fmt.Println("counting")

	for i := 0; i < 2; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}

// Output: 
// counting
// done
// 1
// 0
```

2. Slice length and capacity: A slice has both a length and a capacity. The length of a slice is the number of elements it contains. The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.

```
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s)

	// Extend its length.
	s = s[:4]
	printSlice(s)

	// Drop its first two values.
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}

```

```
len=6 cap=6 [2 3 5 7 11 13]
len=0 cap=6 []
len=4 cap=6 [2 3 5 7]
len=2 cap=4 [5 7]
```

### In-class Notes ###
