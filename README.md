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

3. Appending to a slice: The append built-in function appends elements to the end of a slice. If it has sufficient capacity, the destination is resliced to accommodate the new elements. If it does not, a new underlying array will be allocated. Append returns the updated slice. It is therefore necessary to store the result of append, often in the variable holding the slice itself:
```
func append(slice []Type, elems ...Type) []Type
slice = append(slice, elem1, elem2)
slice = append(slice, anotherSlice...)
```

4. Function closures: A closure is a function value that references variables from outside its body. The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables.
```
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}
```


**Exercises**

1. Exercise: Slices
```
package main

import "golang.org/x/tour/pic"

func Pic(dx, dy int) [][]uint8 {
	pixel := make([][]uint8, dy)
	data := make([]uint8, dx)
	
	for i := range pixel{
		for j := range data{
			data[j] = uint8(i^j)
		}
		pixel[i] = data
	}
	return pixel
}

func main() {
	pic.Show(Pic)
}
```
2. Exercise: Maps
```
package main

import (
	"golang.org/x/tour/wc"
	"strings"
)

func WordCount(s string) map[string]int {
	m := make(map[string]int)
	wordList := strings.Fields(s)
	for i := range wordList{
		m[wordList[i]]++
	}
	return m
}

func main() {
	wc.Test(WordCount)
}
```
3. Exercise: Fibonacci closure
```
package main

import "fmt"

// fibonacci is a function that returns
// a function that returns an int.
func fibonacci() func() int {
	prev := 0
	curr := 1
	i := 0
	return func() int{
		switch i{
			case 0:
				i++
				return prev
			case 1:
				i++
				return curr
			default:
				i++
				tmp := prev + curr
				prev = curr
				curr = tmp
				return curr
		}
	}
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}
```


### In-class Notes ###
