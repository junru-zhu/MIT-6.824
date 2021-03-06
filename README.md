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

5. Interface: An interface type is defined as a set of method signatures. A value of interface type can hold any value that implements those methods.
```
package main

import (
	"fmt"
	"math"
)

type Abser interface {
	Abs() float64
}

func main() {
	var a Abser
	f := MyFloat(-math.Sqrt2)
	v := Vertex{3, 4}

	a = f  // a MyFloat implements Abser
	a = &v // a *Vertex implements Abser

	// In the following line, v is a Vertex (not *Vertex)
	// and does NOT implement Abser.
	a = v

	fmt.Println(a.Abs())
}

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
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

4. Exercise: Stringers
```
package main

import "fmt"

type IPAddr [4]byte

// TODO: Add a "String() string" method to IPAddr.
func (ip IPAddr) String() string {
	return fmt.Sprintf("%v.%v.%v.%v", ip[0], ip[1], ip[2], ip[3])
}

func main() {
	hosts := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for name, ip := range hosts {
		fmt.Printf("%v: %v\n", name, ip)
	}
}

```

5. Exercise: Errors
```
package main

import (
	"fmt"
)

type ErrNegativeSqrt float64

func (e ErrNegativeSqrt) Error() string{
	return "cannot Sqrt negative number: " + fmt.Sprint(float64(e))
}

func Sqrt(x float64) (float64, error) {
	if x >= 0{
		z := 1.0
		i := 0
		for ; z*z < x || i < 10; z -= (z*z - x) / (2 * z) {
			i++
		}
		return z, nil
	}

	return x, ErrNegativeSqrt(x)
}

func main() {
	fmt.Println(Sqrt(2))
	fmt.Println(Sqrt(-2))
}
```

6. Exercise: Readers
```
package main

import "golang.org/x/tour/reader"

type MyReader struct{}

// TODO: Add a Read([]byte) (int, error) method to MyReader.
func (r MyReader) Read(bytes []byte) (int, error){
	for i:= range bytes{
		bytes[i] = 65
	} 
	return len(bytes), nil
}

func main() {
	reader.Validate(MyReader{})
}

```

7. Exercise: Equivalent Binary Trees
```
package main

import (
	"golang.org/x/tour/tree"
	"fmt"
)

// Walk walks the tree t sending all values
// from the tree to the channel ch.
func Walk(t *tree.Tree, ch chan int){
	if t == nil{
		return
	}
	Walk(t.Left, ch)
	ch <- t.Value
	Walk(t.Right, ch)
}

// Same determines whether the trees
// t1 and t2 contain the same values.
func Same(t1, t2 *tree.Tree) bool{
	ch1 := make(chan int, 10)
	ch2 := make(chan int, 10)
	go Walk(t1, ch1)
	go Walk(t2, ch2)
	for i := 0; i < 10; i++ {
		if (<-ch1) != (<-ch2){
			return false
		}
	}
	return true
}

func main() {
	fmt.Println(Same(tree.New(1), tree.New(2)))
	return
}
```
I wrote this code on my own. It is the first time that I feel Go is quite a charming programming language. I feel that I fall in love with this language now!


### In-class Notes ###
