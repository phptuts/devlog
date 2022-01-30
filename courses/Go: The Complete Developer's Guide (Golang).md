# Go: The Complete Developer's Guide (Golang)

## Why?

I am learning golang to be able write Go code to edit the [Arduino Create Agent](https://github.com/arduino/arduino-create-agent) for my [ElectroBlocks](https://electroblocks.org/) site.

https://www.udemy.com/share/101XnU3@XVbdIW28cAi9LF9G-QG9nbsPJ92CupCglAkeLJOk077UbjSrcDk7dlR7XyxyVfg=/

## How to run GO program

```bash
go run file.go
```

## Simple HelloWorld Code

- package: a bunch of code grouped together
- the main package always produces executable code.  Everyother name is a library
- import is how you import libraries and functiosn into your code
- Every package main has a main function which is the entry point for all the code.

```go

package main

import "fmt"

func main() {
	fmt.Println("Hi there!")
}

```

## How to declare slice

- Slices can shrink and grow size wise
- Slices can only store one type of value

```go
colors := []string { "red", "green", "blue" }
```

## How to loop through a slice

```go
colors := []string { "red", "green", "blue" }

for index, color in color {
   fmt.Println(i, "index")
   fmt.Println(color, "value")
}

// If you don't need index or color use an "_"

for _, color in color {
   fmt.Println(color, "value")
}

```

## How to declare alias type

The deck type now has all functionality that a slice of strings would have.

```go
type deck []string
```

## How to add recieve functions 

This creates a receiver function.  

- d is the same as "self" or "this" in other programming languages

```swift
func (d deck) print() {
	for i, card := range d {
		fmt.Println(i, card)
	}
}

var deck = deck{"ace of spades", "queen of hearts"}

deck.print()

```

## How to have a function return multiple values

```go

func math(a int, b int) (int, int) {
   return a + b, a * b
}

add, times := math(3, 4)

// add = 7
// times = 12

```

