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

