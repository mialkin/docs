# Go syntax

[↑ Learn Go in Y minutes](https://learnxinyminutes.com/go/).

## Table of contents

- [Go syntax](#go-syntax)
  - [Table of contents](#table-of-contents)
  - [Entry point](#entry-point)
  - [Comments](#comments)

## Entry point

The top line of every Go file starts with the package declaration. `main` is a special package — this is where your program begins.

The entry point to your application is always the `main` function in the `main` package.

You import packages by using the `import` keyword. It needs to be at the top of the file after the package declaration. The `fmt` (short for "format") is one of the packages from the standard library that comes with Go. It is used to format text and display it.

```go
package main

import "fmt"

func main() {
  fmt.Println("Hello, World!")
}
```

## Comments

Go uses two types of comments:

```go
// This is a single-line comment
```

```go
/*
    This is a
    multi-line comment
*/
```
