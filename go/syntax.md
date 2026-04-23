# Go syntax

[↑ The Go Programming Language Specification](https://go.dev/ref/spec).

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
    - [Multiple returns](#multiple-returns)
    - [Returning errors](#returning-errors)
    - [Error handling](#error-handling)
    - [Error handling (short form)](#error-handling-short-form)
    - [`panic`](#panic)
    - [Skipping unused variable](#skipping-unused-variable)
    - [Variadic functions](#variadic-functions)
  - [Collections](#collections)
    - [Arrays](#arrays)
    - [Slices](#slices)
    - [`len`](#len)
    - [`append`](#append)
    - [Maps](#maps)
    - [`range`](#range)
    - [`break`](#break)
    - [`continue`](#continue)
  - [Control flow](#control-flow)
    - [`if`](#if)
    - [`else`](#else)
    - [`switch`](#switch)
    - [`for`](#for)
  - [Structs](#structs)
    - [Methods](#methods)

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

### Multiple returns

Functions can return any number of values. You need to add another pair of parentheses to define such functions. Separate the types with commas `,`:

```go
func FullName() (string, string) {
  return "Alice", "Smith"
}
```

```go
func MonthAndYear() (string, int) {
  month := "April"
  year := 2012
  return month, year
}
```

### Returning errors

In Go, errors are values returned from the function. If a function returns more than one value, the last one is usually the error, with the type `error`. A `nil` value means no error.

Create new errors with `errors.New()` from the `errors` package:

```go
func CheckUser(username string) error {
  if UserExists(username) {
      return nil
    } else {
      return errors.New("user not found")
    }
}
```

If a function returns multiple values and an error occurs, other arguments are usually returned as zero-value:

```go
func JoinName(firstName string, lastName string) (string, error) {
  if firstName == "" || lastName == "" {
    return "", errors.New("missing first name or last name")
  }

  return firstName + " " + lastName, nil
}
```

### Error handling

To tell if a function was successful, you have to check the error value:

```go
err := EmailExists("alice@example.com")
if err != nil {
  fmt.Println("Error:", err)
}
```

In Go, you will see this snippet a lot:

```go
if err != nil {
  return err
}
```

This means the error is returned again, propagated "up the stack". This can happen multiple times, until one handler decides what to do with it.

One way of handling an error is simply logging it:

```go
if err != nil {
  fmt.Println(err)
}
```

Other times, you might choose to quit the program or show the error to the user.

### Error handling (short form)

The usual error handling is a bit verbose.

```go
err := SendRequest()
if err != nil {
  return err
}
```

If you'd like to make this code more concise, you can use a short form:

```go
if err := SendRequest(); err != nil {
  return err
}
```

Important note: the function's return values are available only in the scope of the `if`. You can't access them outside the `if` statement or its braces. This form is mostly useful for functions that return no other values, just an `error`.

### `panic`

In any moment, you can use the panic function to immediately stop your application with an error message:

```go
panic("something terrible happened")
```

A more useful message usually comes from an error:

```go
if err != nil {
  panic(err)
}
```

A panic prints out a stack trace.

Most of the time, your code shouldn't `panic`. Sometimes it comes handy, like when writing a quick program to validate an idea.

### Skipping unused variable

Sometimes, you don't care about the returned value, just if the result was successful.

A common pattern is to use the `_` identifier to skip a variable:

```go
_, err := OpenFile("results.csv")

if err != nil {
  fmt.Println("Failed to open file")
}
```

It's not optional. Go won't compile code with declared but unused variables.

### Variadic functions

Functions with arbitrary number of arguments are called _variadic_. An example of a variadic function is the `fmt.Println` function.

The list of arguments is marked with three dots `...` before the type's name:

```go
func ShowNames(names ...string) {

}
```

The function can take other arguments as well, but they need to come before the variadic list.

```go
func ShowNames(howMany int, names ...string) {

}
```

The argument becomes a regular slice inside the function:

```go
func PrintCount(names ...string) {
  fmt.Println("Found", len(names), "names")
}
```

You can "unpack" a slice of the same type into variadic arguments using the same three dots syntax.

```go
letters := []string{"a", "b", "c"}
PrintLetters(letters...)
```

In a sense, variadic arguments are no different from passing a slice to a function. It's simply a more convenient way to call the function, as you don't need to create the slice before.

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

You can select a part of a slice by specifying two indexes. It will select elements between the indexes, omitting the last one:

```go
letters := []string{"A", "B", "C", "D", "E", "F"}

fmt.Println(letters[2:4]) // C, D
```

If you omit the first index, it defaults to 0:

```go
fmt.Println(letters[:4]) // [A B C D]
```

If you omit the last index, it defaults to the number of elements (the slice's length):

```go
fmt.Println(letters[4:]) // [E F]
```

If you omit both indexes, you get the entire slice:

```go
fmt.Println(letters[:]) // [A B C D E F]
```

### `len`

The `len` function returns the current length of a slice or array.

```go
methods := []string{"GET", "POST", "DELETE"}
numberOfMethods := len(methods) // 3
```

### `append`

To add a new element to a slice, use the `append` function.

The first argument of append is the slice to append to. Then, any number of arguments that should be appended at the end.

`append` returns the result slice. It doesn't modify the passed slice in-place, so you have to assign the result to a variable:

```go
colors := []string{}

colors = append(colors, "red")
colors = append(colors, "blue", "green")

// colors is equal to []string{"red", "blue", "green"}
```

`append` is a [variadic](#variadic-functions) function. A handy pattern to join two slices is by using the `append` function and _unpacking_ one of them:

```go
newColors := []string{"orange", "turquoise"}

colors = append(colors, newColors...)
```

You can use append together with indexing to do other operations on slices, like removing elements:

```go
withoutThirdElement := append(names[:2], names[3:]...)
```

### Maps

Use maps to store key-value pairs. Map declaration uses brackets with the keys type inside it, followed by the values type.

```go
// A map of strings to ints
var limits map[string]int
```

A declared map is equal to nil. Accessing it in this state will crash your application.

Maps have to be initialized using make:

```go
limits = make(map[string]int)
```

Or using the `:=` syntax and a `map` literal:

```go
limits := map[string]int{}
```

You can assign values to initialized maps:

```go
limits["create"] = 10
```

You can fill maps with values when initializing them:

```go
limits := map[string]int{
  "create": 10,
  "update": 15,
  "delete": 1,
}
```

To retrieve values, use a syntax similar to accessing elements of a slice:

```go
update := limits["update"]
```

If a key doesn't exist, the map returns a zero value, depending on the type (like `0` for integers, `""` for strings, or `nil` for pointers).

It's often important to know if a value exists at all. You can use a special two-value syntax for this:

```go
update, ok := limits["update"]
if !ok {
  // update limits not found
}

fmt.Println("Updates limited to", update)
```

If you just need to check if a key exists, you can skip the value altogether using the underscore `_`:

```go
_, ok := limits["updates"]
if !ok {
  // update limits not found
}
```

To delete a key-value pair, use the delete keyword. If the key doesn't exist, it has no effect.

```go
delete(limits, "update")
```

### `range`

To iterate over collections, Go provides the `range` keyword. It has two forms.

The first uses one variable and assigns to it the index in each iteration:

```go
phoneTypes := []string{"home", "mobile", "work"}

for i := range phoneTypes {
  fmt.Println("Index", i)
  p := phoneTypes[i]
  fmt.Println("Call my", p, "number")
}
```

The second form uses two variables. The first variable is the index, the second one is the current element of the slice:

```go
for i, p := range phoneTypes {
  fmt.Println("Index", i)
  fmt.Println("Call my", p, "number")
}
```

Most of the time, you won't need the index in the second form. You can just skip it using the `_` identifier:

```go
for _, p := range phoneTypes {
  fmt.Println("Call my", p, "number")
}
```

Similarly to slice ranges, you can use `range` to iterate over maps.

It also comes in two flavors. The one-value range goes over the keys in a map:

```go
interfaceColors := map[string]string{
  "alice": "blue",
  "bob":   "orange",
  "emma":  "green",
}

for person := range interfaceColors {
  color := interfaceColors[person]
  fmt.Println(person, "uses", color, "interface")
}
```

The two-values range goes over key-value pairs in a map:

```go
for person, color := range interfaceColors {
  fmt.Println(person, "uses", color, "interface")
}
```

A key difference between slices and maps is that maps are not ordered. Calling `range` on a map returns the values in a random order, so you can't rely on it.

### `break`

Use `break` to interrupt the loop immediately:

```go
found := false

for _, name := range names {
  if name == targetName {
    found = true
    break
  }
}
```

### `continue`

Use `continue` to skip the current loop's iteration and go to the next:

```go
for _, status := range statuses {
  if status == "disabled" {
    continue
  }
}
```

## Control flow

### `if`

To make decisions, use the if statement and comparison operators.

```go
if speedInKnots >= 64 {
  // A Hurricane!
}
```

```go
if role == "admin" {
  // Access granted
}
```

### `else`

Use `else` to execute code when an `if` condition is not met:

```go
if role == "admin" {
  // Access granted
} else {
  // Access denied
}
```

You can combine any number of conditions with `else if`:

```go
if role == "admin" {
  // Admin access granted
} else if role == "user" {
  // User access granted
} else {
  // Access denied
}
```

### `switch`

```go
switch x {
case 0:
  // Zero
case 1:
  // One
case 2:
  // Two
default:
  // Other
}
```

### `for`

Go has one loop — `for`.

Using `for` without arguments creates an infinite loop:

```go
for {
  // Infinite loop
}
```

The loop bellow will run ten times, as `x` goes from `0` to `9`:

```go
Copy to clipboardx := 0
for x < 10 {
  fmt.Println("x =", x)
  x++
}
```

There's also a short form of this approach:

```go
for x := 0; x < 10; x++ {
  fmt.Println("x =", x)
}
```

## Structs

Struct are groups of variables. The variables held by a struct are called "fields". They let you treat many values as a single unit:

```go
type Money struct {
  Amount   int
  Currency string
}
```

You can create a struct with all fields set to zero-values:

```go
m := Money{}
```

Or you can fill in all or some of them:

```go
m := Money{
  Amount:   10,
  Currency: "USD",
}
```

To access a field, use the struct variable followed by a dot `.` and the field name:

```go
amount := m.Amount
m.Currency = "EUR"
```

Structs come especially useful for passing a group of variables as a single argument:

```go
type Money struct {
  Amount   int
  Currency string
}

func Format(m Money) string {
  return fmt.Sprintf("%v %v", m.Amount, m.Currency)
}
```

You can initialize structs without field names, passing values in the order they are defined:

```go
func main() {
  m := Money{100, "USD"}
  fmt.Println(Format(m))
}
```

### Methods

Methods are functions bound to a single struct. To write a method, write a regular function, but add an extra section in parentheses before its name. It should contain a name and the type:

```go
type Money struct {
  Amount   int
  Currency string
}

func (m Money) Format() string {
  return fmt.Sprintf("%v %v", m.Amount, m.Currency)
}
```

The receiver part (`(m Money)`) is how you reference the variable inside the method. The name is usually a one letter and should be the same for all methods on a single type.

When you consider the previous example, it's clear that methods are just a more convenient way to write functions working with a struct:

```go
func Format(m Money) string {
  return fmt.Sprintf("%v %v", m.Amount, m.Currency)
}

func (m Money) Format() string {
  return fmt.Sprintf("%v %v", m.Amount, m.Currency)
}
```

You call methods similarly to regular functions. Prefix the method name with the struct variable and a dot `.`:

```go
formattedByFunction := Format(m)

formattedByMethod := m.Format()
```
