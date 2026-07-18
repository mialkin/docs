# Go syntax

[↑ The Go Programming Language Specification](https://go.dev/ref/spec).

[↑ Go Proverbs](https://go-proverbs.github.io/).

[↑ Learn Go in Y minutes](https://learnxinyminutes.com/go/).

[↑ Effective Go](https://www.math.bas.bg/softeng/bantchev/place/go/effective-go.pdf).

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
    - [Anonymous functions](#anonymous-functions)
    - [Closures](#closures)
    - [Named returns](#named-returns)
    - [`defer`](#defer)
    - [`recover`](#recover)
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
    - [Pointer receivers](#pointer-receivers)
    - [Tags](#tags)
    - [Exported fields](#exported-fields)
    - [Constructors](#constructors)
  - [Pointers](#pointers)
    - [Passing pointers](#passing-pointers)
    - [`nil`](#nil)
    - [`new`](#new)
  - [Interfaces](#interfaces)
  - [Modules](#modules)
    - [`go get`](#go-get)
  - [Packages](#packages)
    - [Exported symbols](#exported-symbols)
  - [Concurrency](#concurrency)
    - [Goroutines](#goroutines)
    - [Channels](#channels)
      - [Channel range](#channel-range)
      - [Closing channels](#closing-channels)
      - [Channel directions](#channel-directions)
      - [`select`](#select)
    - [Locks](#locks)

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

Such a variable has no value assigned yet:

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

The same thing works for other operators:

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

You can also declare _global variables_ outside of functions. They have their uses but are rarely needed — stick to local variables when you can.

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

The argument becomes a regular [slice](#slices) inside the function:

```go
func PrintCount(names ...string) {
  fmt.Println("Found", len(names), "names")
}
```

You can "unpack" a slice of the same type into variadic arguments using the same three-dot syntax.

```go
letters := []string{"a", "b", "c"}
PrintLetters(letters...)
```

In a sense, variadic arguments are no different from passing a slice to a function. It's simply a more convenient way to call the function, as you don't need to create the slice before.

You can use them in function arguments like any other type.

### Anonymous functions

A function stored as a value is called an **anonymous function**.

```go
func main() {
	add := func(x int, y int) int {
		return x + y
	}

	multiply := func(x int, y int) int {
		return x * y
	}

	Calculate(add)
	Calculate(multiply)
}

func Calculate(f func(int, int) int) {
	result := f(10, 50)
	fmt.Println(result)
}
```

If anonymous functions are values, they can also be returned from other functions:

```go
func NewGreeter() func() {
	return func() {
		fmt.Println("Hello!")
	}
}

func main() {
	greeter := NewGreeter()
	greeter()
}
```

### Closures

**Closures** are [anonymous functions](#anonymous-functions) that use local variables outside their scope.

This is useful for keeping a variable that's shared across the function's calls.

```go
func NewCounter() func() int {
	counter := 0
	return func() int {
		counter += 1
		return counter
	}
}

func main() {
	counter := NewCounter()

	a := counter() // 1
	b := counter() // 2
	c := counter() // 3
}
```

### Named returns

Go allows naming the return values.

```go
func Add(x int, y int) (sum int) {
	sum = x + y
	return
}
```

They act as normal local variables. You still need to write the empty `return` statement to exit from the function.

Since they're already declared, you use `=`, not `:=` to assign values to them.

### `defer`

The `defer` keyword schedules a function to run when the current function returns. No matter how the function exits (normal return, early return, or panic) the deferred function always runs.

```go
func ReadFile(path string) error {
	file, err := os.Open(path)
	if err != nil {
		return err
	}
	defer file.Close() // Runs when ReadFile returns, guaranteed

	// Work with file...
	return nil
}
```

This pattern is common for cleanup: closing files, releasing locks, or recording metrics.

The deferred function runs **after** the return value is set but **before** the function actually returns. You can inspect the result and act on it, automatically, for every return path.

```go
func Execute(f func() error, metrics *Metrics) (err error) {
	metrics.StoreExecution()

	defer func() {
		if err == nil {
			metrics.StoreSuccess()
		} else {
			metrics.StoreFailure()
		}
	}()

	// No matter how many returns we add later,
	// the defer always runs with the final err value
	return f()
}
```

No matter how many `return` statements exist or where they are, the deferred function always runs with the final value of `err`. You cannot forget to call it. Notice the parentheses (`()`) after the anonymous function's body.

You can call `defer` multiple times. The last deferred function is the first that's going to be called.

```go
func Execute() {
	defer third()
	defer second()
	defer first()
}
```

### `recover`

The `recover` function stops a `panic` from crashing your application. You must call it inside a `defer` block, or it has no effect.

`recover` only prevents panics in the function where it's called. It won't catch panics in other functions or goroutines.

```go
func Example() {
	defer func() {
		r := recover()
		if r != nil {
			fmt.Println("Recovered panic:", r)
		}
	}()

	panic("boom")
}
```

An unrecovered panic in a goroutine will crash the entire program, not just that goroutine. Each goroutine must have its own `recover` if you want to handle panics—you can't recover a panic from one goroutine in another.

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

You can use `append` together with indexing to do other operations on slices, like removing elements:

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

If a key doesn't exist, the map returns a zero value, depending on the type (like `0` for integers, `""` for strings, or [`nil`](#nil) for [pointers](#pointers)).

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

To delete a key-value pair, use the `delete` keyword. If the key doesn't exist, it has no effect.

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

Structs are groups of variables. The variables held by a struct are called "fields". They let you treat many values as a single unit:

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

You call methods much like regular functions. Prefix the method name with the struct variable and a dot `.`:

```go
formattedByFunction := Format(m)

formattedByMethod := m.Format()
```

### Pointer receivers

Just as with other types, you can pass a [pointer](#pointers) to a struct to modify it inside a function.

```go
func ChangeToZero(m *Money) {
  m.Amount = 0
}

func main() {
  m := &Money{}
  ChangeToZero(m)
}
```

Similarly, methods allow modifying structs in place:

```go
func main() {
  m := &Money{}
  m.ChangeToZero()
}
```

To do this, add an asterisk `*` to the receiver type. It lets you modify the struct fields inside methods:

```go
func (m *Money) ChangeToZero() {
  m.Amount = 0
}
```

### Tags

A common use case for structs is transforming them into text (called _marshaling_ or _serializing_). By default, `encoding/json` package bases key names on the struct fields. You can use _tags_ to control this.

Tags are string literals used as metadata on fields. Define them within backticks so you can use the standard double-quotes inside:

```go
type User struct {
  Name       string `json:"name"`
  YearJoined int    `json:"year_joined"`
  Premium    bool   `json:"premium"`
}
```

Here is the marshalling:

```go
func main() {
  alice := User{
    Name:       "Alice",
    YearJoined: 2022,
    Premium:    true,
  }

  marshaledUser, _ := json.Marshal(alice)
  fmt.Println(string(marshaledUser))
}
```

The output of marshaling this struct would be:

```json
{
  "name": "Alice",
  "year_joined": 2022,
  "premium": true
}
```

### Exported fields

Only fields starting with an uppercase letter can be used outside the package they're in. We call them exported. Fields starting with a lowercase letter (_unexported_) aren't visible for other packages.

Consider the `json.Marshal`. The `encoding/json` package won't be able to access unexported fields on any struct you pass to its functions:

```go
type Payload struct {
  Source          string // exported
  internalDetails string // unexported
}
```

### Constructors

You can introduce more safety by making the struct's fields unexported and creating a dedicated _constructor_.

A constructor is a function that returns an initialized struct:

```go
type Money struct {
  amount   int
  currency string
}

func NewMoney(amount int, currency string) Money {
  return Money{
    amount:   amount,
    currency: currency,
  }
}
```

Whoever is using your package needs to use the constructor to create a new struct instance:

```go
twentyDollars := money.NewMoney(20, "USD")
```

## Pointers

A **pointer** is a variable that points to another variable.

To declare a pointer, add an asterisk `*` before the type. The type is crucial: a pointer can only point to variables of the defined type.

```go
alice := "Alice"
bob := "Bob"

var current *string
current = &alice
```

To access the actual variable, add an asterisk `*` before the variable. It's called _dereferencing_:

```go
fmt.Println(current)  // Prints: 0x14000010290
fmt.Println(*current) // Prints: Alice
```

You can change what the pointer points to at any time:

```go
current = &bob
```

Pointers can be used as other types: as struct fields or in collections.

Pointers always refer to the type immediately to the right of the asterisk `*` symbol:

```go
// a map of string to pointers to integers
var counters map[string]*int
// a slice of pointers to booleans
var choices []*bool
// a pointer to slice of strings
var dynamicList *[]string
```

### Passing pointers

If a function modifies its arguments, it doesn't really access the original variable. What it changes is a _copy_ of the passed variable:

```go
func Increase(number int) {
  number++
}

func main() {
  x := 0
  Increase(x)
  // x is still 0
}
```

Pointers let you modify the actual passed variable:

```go
func Increase(number *int) {
  *number++
}

func main() {
  x := 0
  Increase(&x)
  // x is now 1
}
```

### `nil`

Declared pointers are equal to their zero-value: `nil`. A `nil` indicates that the pointer doesn't point to anything. You can use this to indicate that a variable wasn't assigned yet:

```go
var isUpdated *bool

if isUpdated == nil {
  fmt.Println("isUpdated not set")
} else {
  fmt.Println("isUpdated set to", *isUpdated)
}
```

An `if` like this is often called a _nil-check_. It's key when working with pointers.

If you try accessing a `nil` pointer's value (also called _dereferencing_) your application will panic.

### `new`

Because a declared pointer is equal to `nil`, you need an extra step to assign it to an empty variable:

```go
var number int
pointer := &number
```

To make it shorter, use the built-in `new` function that creates a zeroed variable and returns a pointer to it:

```go
pointer := new(int)
// *pointer is equal 0
```

You can also use `new` to initialize structs:

```go
alice := new(User)

// Same thing
bob := &User{}
```

## Interfaces

Interfaces let you specify what methods you need, without relying on the exact struct.

```go
type Storage interface {
	Store(user User)
}

type User struct {
	Name string
}

func NewUser(name string, storage Storage) User {
	user := User{Name: name}
	storage.Store(user)
	return User{}
}

type MapStorage struct {
	users map[string]User
}

func (m MapStorage) Store(user User) {
	m.users[user.Name] = user
}
```

## Modules

Modules are complete Go projects, like a library or an application. Usually, a single repository keeps a single module.

A module is defined in a `go.mod` file. You don't need to modify this file.
The `go` CLI manages it for you.
To generate the `go.mod` file, run `go mod init` providing the module's name:

```bash
go mod init my-app
```

It generates content like this:

```text
module my-app

go 1.23
```

The module's name is usually the full path to the repository. For example, `github.com/john-doe/foo-bar`.

It's possible to use any name you like, but it won't be seamless to import your module later. As a rule of thumb, use the full path of your repository as the module's name.

### `go get`

To import an external module, you have to add it as a dependency.

To do so, run the `go get` command followed by the module's full path.

For example:

```bash
go get github.com/google/uuid
```

This will change your `go.mod` file to something like:

```text
module tax

go 1.23

require github.com/google/uuid v1.3.0 // indirect
```

It will also generate a `go.sum` file with checksums of the dependencies.
You won't need to modify this file.
If you use version control (like a git repository), commit both `go.mod` and `go.sum` files.

## Packages

You can group `.go` files in directories. Go treats each directory as a **package**.
All files in the directory need to have the same `package` at the top.

```go
package storage
```

To import a package, add an import path using the full [module](#modules)'s path.

```go
import "my-app/storage"
```

To call functions or access variables from the package, use its name.

```go
users, err := storage.AllUsers()
```

The package name doesn't need to be the same as the directory name.
But it's confusing otherwise, so name packages the same as the directories that keep them.

The difference between [modules](#modules) and packages:

- One module can contain many packages (directories)
- You need only one `go.mod` file in one module (in the top directory)
- Modules are defined by `go.mod`, packages are defined by directories

### Exported symbols

You can only import symbols (variables, functions, types) from other packages if they start with an uppercase letter.

```go
func ExportedFunction() {

}

func unexportedFunction() {

}

const ExportedConst = 100

var unexportedVariable = "example"

type ExportedType struct {

}
```

Aim to export only what external packages would use. Make all internal details unexported.

Similar rule applies to structs. Field names starting with lowercase won't be visible outside the package.

```go
type Money struct {
	amount int
	currency string
}

func (m Money) String() string {
	return fmt.Sprint(m.amount, " ", m.currency)
}
```

External packages are able to access the `String()` method, but not the `amount` or `currency` fields.

## Concurrency

### Goroutines

**Goroutines** are functions that run in the background.

To start a goroutine, add the `go` keyword when calling a function.

```go
func main() {
	// Run in the background
	go Greet("World")

	Greet("Mike")
}

func Greet(who string) {
	fmt.Println("Hello,", who)
}
```

The first `Greet` function is executed _concurrently_ to the `main` function. It means that `main` will continue to run, not waiting for it to finish.

If you run the snippet above, the result will most likely be:

```bash
Hello, Mike
```

This is because the application ends after the `main` function has finished, and the goroutine we started might not have a chance to execute.

To see both results, we can add `time.Sleep()`. It will block the current function for the given time.

```go
func main() {
	// Run in the background
	go Greet("World")

	Greet("Mike")

	time.Sleep(time.Millisecond)
}
```

Now, the results will likely be:

```bash
Hello, Mike
Hello, World
```

"Likely", because the results may vary, depending on which goroutine is executed first.

When working with goroutines, keep in mind they are _concurrent_.
You can't determine the order in which they are executed (at least without using more advanced constructs).

### Channels

[Goroutines](#goroutines) don't know about each other. The functions running as goroutines can't return values in the traditional way, as the caller doesn't wait for them to finish.

The standard way to communicate between goroutines is using _channels_.

A **channel** is a type that lets you send and receive values of a specific type.
You need to initialize it with `make`, similarly to maps.

```go
var users chan string
users = make(chan string)

// Same thing
users := make(chan string)
```

To send and receive values, use the `<-` operator. When sending, the channel goes on the left side. When receiving, on the right.

```go
// Send to the users channel
users <- "Alice"
users <- "Bob"

// Retrieve from the users channel
nextUser := <-users
```

The `chan` keyword is always followed by a type. If you pass channels as arguments, remember to specify it:

```go
func AddUser(users chan string) {
	users <- "Alice"
}
```

#### Channel range

As with slices and maps, you can use `for` and `range` to go over values in a channel.

```go
luckyNumbers := make(chan int)
for number := range luckyNumbers {
	fmt.Println("Your next lucky number is", number)
}
```

Such loop blocks until there's a value to be received from the channel.

You can use `break` and `continue` like in other `for` loops.

#### Closing channels

A channel can be _closed_ using `close`.

```go
close(luckyNumbers)
```

Closing a channel interrupts any loops waiting for more messages.

Reading from a closed channel returns immediately, returning the zero-value of the channel's type.

```go
// Always zero after the channel is closed
number := <-luckyNumbers
```

You can also check if a channel is closed using the two-value syntax:

```go
number, ok := <-luckyNumbers
if ok {
	fmt.Println("Your lucky number is", number)
} else {
	fmt.Println("Out of lucky numbers")
}
```

Don't write to closed channels — it panics.

#### Channel directions

When using channels as function arguments, you can decide if the channel should allow reads or writes.

```go
func readAndWrite(ch chan string) {

}

func readOnly(ch <-chan string) {

}

func writeOnly(ch chan<- string) {

}
```

This has an impact on the channel only within the function.

#### `select`

When you need to read from multiple channels at the same time, use `select`.

`select` is similar to `switch` but it works with channels. It blocks until one of the `case` statements can be executed.

When reading from multiple channels, `select` executes the `case` that has values ready to be received.
If more than one `case` can be executed, `select` picks one randomly.

```go
select {
case command := <-commands:
	// Execute a command
case err := <-errors:
	// Handle an error
}
```

A common use-case is using `time.After()` to introduce a timeout.

```go
select {
case command := <-commands:
	// Execute a command
case <-time.After(time.Second):
	// Timed out waiting for a command
}
```

A `default` case is executed if no other actions are possible.

An empty `default` case is a way to continue immediately if there are no values to be received from a channel.

```go
ready := make(chan bool)

// Blocks until ready
<-ready

// Checks if ready and continues otherwise
select {
case <-ready:
	fmt.Println("ready")
default:
	fmt.Println("not ready yet")
}
```

`select` is often useful in combination with `for`.

For example, the snippet below infinitely checks until a value can be received from a channel, with 1-second sleep between checks.

```go
func WaitUntilReady(ready chan bool) {
	for {
		select {
		case <-ready:
			fmt.Println("ready")
			return
		default:
			fmt.Println("not ready yet")
			time.Sleep(time.Second)
		}
	}
}
```

Warning: using `break` in a select statement interrupts the `select`, not the `for`.
In this scenario, it's best to use a separate function and a `return` instead.

An empty `select` waits forever. Similarly to an empty `for`, but is much easier on the CPU.

```go
select {}
```

### Locks

Another way to synchronize [goroutines](#goroutines) is using a **mutex**, also known as a **lock**.
It's a simpler concept than [channels](#channels).

Go provides locks as [`sync.Mutex`](https://pkg.go.dev/sync#Mutex). When you call its `Lock()` method, all subsequent `Lock()` calls will block until `Unlock()` is called.

Locks come helpful when multiple goroutines access a single resource that is not _thread-safe_ (i.e., can't be used by multiple goroutines at once).

A common example is a `map`. If multiple goroutines try to write to the same map, you'll get a panic:

```bash
fatal error: concurrent map writes
```

Another example could be a file on disk. If multiple goroutines try to write to a file, you might end up with inconsistent data.

To avoid these problems, use locks around the code that uses the common resource:

```go
var (
	lock sync.Mutex
	metrics = map[string]int{}
)

func SaveMetric(name string, value int) {
	lock.Lock()

	metrics[name] = value

	lock.Unlock()
}
```

When you need to handle errors, you need to be very careful to call `Unlock()` in every scenario.
If you miss one, you might end up with a deadlock and your application will freeze or crash.

This makes `defer` shine when used together with locks. Simply follow the `Lock()` call with a deferred `Unlock()`.
Whatever happens in the rest of the function, the lock releases automatically.

```go
func SaveMetricsToFile(content []byte) error {
	lock.Lock()
	defer lock.Unlock()

	return os.WriteFile("metrics.csv", content, 0644)
}
```

The `sync.Mutex`'s zero-value is ready to use.

You can't copy a `Mutex` after it's initialized. In Go, structs are copied when passed as functions arguments.
If you pass a `Mutex` to a function or keep it as a struct field, store it as a pointer.

Remember that the zero-value of `*sync.Mutex` is `nil`, so you have to initialize it in this case.

```go
type MetricSaver struct {
	lock *sync.Mutex
}

func (m MetricSaver) SaveMetricsToFile(content []byte) error {
	m.lock.Lock()
	defer m.lock.Unlock()

	return os.WriteFile("metrics.csv", content, 0644)
}

func main() {
	saver := MetricSaver{
		lock: &sync.Mutex{},
	}

	_ = saver.SaveMetricsToFile([]byte("my metrics"))
}
```

Use constructors to initialize structs like this.
