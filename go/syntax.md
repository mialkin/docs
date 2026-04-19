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
  - [`var` blocks](#var-blocks)
  - [Globals](#globals)
  - [Functions](#functions)
  - [Collections](#collections)
    - [Arrays](#arrays)
    - [Slices](#slices)

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

## `var` blocks

There's a short syntax for declaring many variables at once. You can use it in both local and [global](#globals) scopes.

```go
var (
  name  = "Alice"
  hours = 10
)
```

The same works for constants:

```go
const (
  pi         = 3.1415
  hoursInDay = 24
)
```

## Globals

When you declare a variable inside a function, it's called a _local variable_. It means other functions can't access it.

You can also declare _global variables_ outside of functions. They have their uses, but are rarely needed — stick to local variables when you can.

```go
package main

import "fmt"

const taxRate = 0.1
var price = 40.0

func main() {
  tax := taxRate * price
  fmt.Println("tax:", tax)
}
```

## Functions

You can define functions before or after the `main` function:

```go
package main

import "fmt"

func main() {
  Greet("Alice")
}

func Greet(name string) {
 fmt.Println("Hello, " + name)
}
```

Functions can return values. You can return either explicit values (like `12`) or a variable with matching type:

```go
func NumberOfMonths() int {
  return 12
}

func DaysInAWeek() int {
  days := 7
  return days
}
```

A function can take any number of arguments, separated by commas:

```go
func Add(x int, y int) int {
  sum := x + y
  return sum
}
```

If arguments next to each other are of the same type, you can use a shorter form:

```go
func Add(x, y int) int {
  return x + y
}
```

## Collections

### Arrays

Arrays keep a set of variables of the same type. They have a fixed size defined within brackets:

```go
var contactMethods [3]string
```

You can initialize an array when declaring it:

```go
contactMethods := [3]string{"email", "phone", "sms"}
```

You can reference array items by index (starting from 0):

```go
// Retrieve array item
email := contactMethods[0]

// Set array item
contactMethods[1] = "mail"
```

### Slices

Most of the time, you won't work with arrays directly. Instead, you'll use _slices_.

You can think of slices as dynamic arrays. You don't declare how many elements they have. They can expand and shrink.

When declaring a slice, omit the size from brackets:

```go
var contacts []string
```

A declared slice is equal to `nil`. `nil` means "nothing" or "no value" — the slice is not initialized yet.

If you try to access elements of a `nil` value, your application will crash:

```go
var contacts []string

// This will crash your application with message:
// panic: runtime error: index out of range [0] with length 0
contacts[0] = "Jenny"
```

You can use the slice literal to initialize an empty slice:

```go
contacts := []string{}
```

Or preallocate some memory for a slice using `make`:

```go
// A slice of five empty strings
contacts := make([]string, 5)
```

Similarly to arrays, you can initialize a slice with a set of values.

```go
contacts := []string{
  "Alice",
  "John",
  "Emma",
}
```
