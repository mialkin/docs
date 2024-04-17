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
    - [`Or`](#or)

## Methods

There are more overloaded [↑ methods](https://learn.microsoft.com/en-us/dotnet/api/system.threading.interlocked#methods) in `Interlocked` class, below listed just subset of them.

### `Add(Int32, Int32)`

Adds two 32-bit integers and replaces the first integer with the sum, as an atomic operation:

```csharp
int location1 = 1;
int value = 2;

Interlocked.Add(ref location1, value);
Console.WriteLine($"{location1}, {value}"); // 3, 2
```

This method handles an overflow condition by wrapping: if the value at `location1` is `Int32.MaxValue` and `value` is `1`, the result is `Int32.MinValue`; if value is `2`, the result is (`Int32.MinValue + 1`); and so on. No exception is thrown:

```csharp
Console.WriteLine($"{Int32.MinValue}, {Int32.MaxValue}"); // -2147483648, 2147483647

int location1 = Int32.MaxValue;;
int value = 2;

Interlocked.Add(ref location1, value);
Console.WriteLine($"{location1}, {value}"); // -2147483647, 2
```

### `And(Int32, Int32)`

Bitwise "ands" two 32-bit signed integers and replaces the first integer with the result, as an atomic operation. Returns the original value in `location1`.

```csharp
int location1 = 12; // 1100
int value = 7;      // 0111

// 1100 AND 0111 = 0100 = 4

int result = Interlocked.And(ref location1,value); // result is the original value in location1.

Console.WriteLine($"{location1}, {value}, {result}"); // 4, 7, 12
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
int location1 = 1;
int value = 2;

int result = Interlocked.Exchange(ref location1, value);

Console.WriteLine($"{result}, {location1}, {value}"); // 1, 2, 2
```

### `CompareExchange(Int32, Int32, Int32)`

Compares two values for equality and, if they are equal, replaces the first value.

Exchange didn't take place:

```csharp
int location1 = 1;  // The destination, whose value is compared with comparand and possibly replaced.
int value = 2;      // The value that replaces the destination value if the comparison results in equality.
int comparand = 3;  // The value that is compared to the value at location1.

int result = Interlocked.CompareExchange(ref location1, value, comparand); // The result is the original value in location1.

Console.WriteLine($"{location1}, {value}, {comparand}, {result}");  // 1, 2, 3, 1
```

If comparand and the value in `location1` are equal, then `value` is stored in `location1`. Otherwise, no operation is performed. The compare and exchange operations are performed as an atomic operation. The return value of `CompareExchange` is the original value in `location1`, whether or not the exchange takes place.

Exchange took place:

```csharp
int location1 = 3;  // The destination, whose value is compared with comparand and possibly replaced.
int value = 2;      // The value that replaces the destination value if the comparison results in equality.
int comparand = 3;  // The value that is compared to the value at location1.

int result = Interlocked.CompareExchange(ref location1, value, comparand); // The result is the original value in location1.

Console.WriteLine($"{location1}, {value}, {comparand}, {result}");  // 2, 2, 3, 3
```

### `Decrement(Int32)`

Decrements a specified variable and stores the result, as an atomic operation.

```csharp
int location = 1;

int result = Interlocked.Decrement(ref location); // Returns the decremented value.

Console.WriteLine($"{location}, {result}"); // 0, 0
```

### `Or`

Bitwise "ors" two 32-bit signed integers and replaces the first integer with the result, as an atomic operation.

```csharp
int location1 = 12; // 1100
int value = 5;      // 0101

// 1100 OR 0101 = 1101 = 13

int result = Interlocked.Or(ref location1, value); // Returns the original value in location1.

Console.WriteLine($"{location1}, {value}, {result}"); // 13, 5, 12
```
