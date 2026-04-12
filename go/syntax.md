# Go syntax

[↑ Learn Go in Y minutes](https://learnxinyminutes.com/go/).

## Table of contents

- [Go syntax](#go-syntax)
  - [Table of contents](#table-of-contents)
  - [Entry point](#entry-point)
  - [Comments](#comments)
  - [Variables](#variables)
  - [Constants](#constants)
  - [Operators](#operators)

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

## Variables

When you first declare a variable, pick a name for it and assign a value using `:=`:

```go
name := "Alice"
year := 2001
```

You can then modify the variable using the `=` operator:

```go
name = "Bob"
```

To declare a variable, use the `var` keyword together with a name and a type:

```go
func main() {
  // Declare
  var daysInDecember int

  // Assign
  daysInDecember = 31

  // Declare and assign
  var pi = 3.1415

  // Also declare and assign
  learningGo := true
}
```

Such variable has no value assigned yet:

```go
var firstName string
```

They're equal to the zero value. It depends on the type. For strings it's an empty string (`""`), for numbers it's `0`, and for booleans it's `false`.

Use `=` to assign values to already declared variables and when using `var`.

Use `:=` when declaring and assigning variable for the first time.

## Constants

If you replace the `var` keyword with `const`, you declare a constant. Constants can't have their value changed once assigned.

```go
const yearOfBirth = 2001
```

## Operators

Go supports all usual math operators like `+`, `-`, `*`, `/`, and `%` (modulo).

```go
ten := 5 + 5
five := 10 / 2
```

You can join strings with `+`.

```go
fullName := firstName + " " + lastName
```

Go supports short forms of incrementing variables:

```go
// Same thing
i = i + 1
i += 1
i++
```

Same thing works for other operators:

```go
counter--
multiplier *= 2
divider /= 10
```
