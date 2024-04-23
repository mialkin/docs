# `Interlocked`

The [↑ `System.Threading.Interlocked`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.interlocked) class provides static methods that help protect against errors that can occur when the scheduler switches contexts while a thread is updating a variable that can be accessed by other threads, or when two threads are executing concurrently on separate processors. The members of this class do not throw exceptions.

The [`Increment`](#incrementint32) and [`Decrement`](#decrementint32) methods increment or decrement a variable and store the resulting value in a single operation. On most computers, incrementing a variable is not an atomic operation, requiring the following steps:

1. Load a value from an instance variable into a register.
2. Increment or decrement the value.
3. Store the value in the instance variable.

If you do not use `Increment` and `Decrement`, a thread can be [preempted](/csharp/concurrency/thread.md#thread-preemption) after executing the first two steps. Another thread can then execute all three steps. When the first thread resumes execution, it overwrites the value in the instance variable, and the effect of the increment or decrement performed by the second thread is lost.

The `Add` method atomically adds an integer value to an integer variable and returns the new value of the variable.

The `Exchange` method atomically exchanges the values of the specified variables. The `CompareExchange` method combines two operations: comparing two values and storing a third value in one of the variables, based on the outcome of the comparison. The compare and exchange operations are performed as an atomic operation.

## Table of contents

- [`Interlocked`](#interlocked)
  - [Table of contents](#table-of-contents)
  - [Methods](#methods)
    - [`Add(Int32, Int32)`](#addint32-int32)
    - [`And(Int32, Int32)`](#andint32-int32)
    - [`Increment(Int32)`](#incrementint32)
    - [`Exchange(Int32, Int32)`](#exchangeint32-int32)
    - [`CompareExchange(Int32, Int32, Int32)`](#compareexchangeint32-int32-int32)
    - [`Decrement(Int32)`](#decrementint32)
    - [`Or(Int32, Int32)`](#orint32-int32)

## Methods

There are more overloaded [↑ methods](https://learn.microsoft.com/en-us/dotnet/api/system.threading.interlocked#methods) in `Interlocked` class, below listed just subset of them.

### `Add(Int32, Int32)`

Adds two 32-bit integers and replaces the first integer with the sum, as an atomic operation. Returns the new value that was stored by this operation.

```csharp
int value = 1;
var result = Interlocked.Add(ref value, 2);

Console.WriteLine(value);
Console.WriteLine(result);

// Output:
// 3
// 3
```

This method handles an overflow condition by wrapping. No exception is thrown:

```csharp
var value = int.MaxValue;
var result = Interlocked.Add(ref value, 2);

Console.WriteLine(int.MinValue);
Console.WriteLine(value);
Console.WriteLine(result);

// Output:
// -2147483648
// -2147483647
// -2147483647
```

### `And(Int32, Int32)`

Bitwise "ands" two 32-bit signed integers and replaces the first integer with the result, as an atomic operation. Returns the original value.

```csharp
var value = 12;

// 1100 AND 0111 = 0100 = 4
var result = Interlocked.And(ref value, 7);

Console.WriteLine(value);
Console.WriteLine(result);

// Output:
// 4
// 12
```

### `Increment(Int32)`

```csharp
public static int Increment(ref int location) => Interlocked.Add(ref location, 1);
```


Increments a specified variable and stores the result, as an atomic operation:

```csharp
var value = 5;
var result = Interlocked.Increment(ref value);

Console.WriteLine(value);
Console.WriteLine(result);

// Output:
// 6
// 6
```

This method handles an overflow condition by wrapping: if `value` = `Int32.MaxValue`, `value + 1` = `Int32.MinValue`. No exception is thrown.

### `Exchange(Int32, Int32)`

Sets a 32-bit signed integer to a specified value and returns the original value, as an atomic operation:

```csharp
var value = 10;
var result = Interlocked.Exchange(ref value, 4);

Console.WriteLine(value);
Console.WriteLine(result);

// Output:
// 4
// 10
```

### `CompareExchange(Int32, Int32, Int32)`

Compares two values for equality and, if they are equal, replaces the first value. Returns the original first value.

```csharp
public static int CompareExchange (ref int location1, int value, int comparand);
```

Exchange doesn't take place because 10 is not equal to 30:

```csharp
var value = 10;

var result = Interlocked.CompareExchange(ref value, 20, 30);

Console.WriteLine(value);
Console.WriteLine(result);

// Output:
// 10
// 10
```

Exchange takes place:

```csharp
var value = 10;

var result = Interlocked.CompareExchange(ref value, 20, 10);

Console.WriteLine(value);
Console.WriteLine(result);

// Output:
// 20
// 10
```

### `Decrement(Int32)`

Decrements a specified variable and stores the result, as an atomic operation.

```csharp
var value = 5;
var result = Interlocked.Decrement(ref value);

Console.WriteLine(value);
Console.WriteLine(result);

// Output:
// 4
// 4
```

### `Or(Int32, Int32)`

Bitwise "ors" two 32-bit signed integers and replaces the first integer with the result, as an atomic operation. Returns the original value as a result.

```csharp
// 1100
var value = 12;

// 5 is 0101 in binary
// 1100 OR 0101 = 1101 = 13
var result = Interlocked.Or(ref value, 5);

Console.WriteLine(value);
Console.WriteLine(result);

// Output:
// 13
// 12
```
