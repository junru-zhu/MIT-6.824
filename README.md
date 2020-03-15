# MIT-6.824

## Lecture 1

## Lecture 2
### Pre-class Assignment: A Tour of Go ###

**Questions**

1. what is the range of numbers represented by float64?<br/>Answer: 

2. what is the significance of the time 2009-11-10 23:00:00 UTC for Go playground?<br/>Answer:

**Notes**

1. Defer: A defer statement defers the execution of a function until the surrounding function returns. Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.

```
func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

### In-class Notes ###
