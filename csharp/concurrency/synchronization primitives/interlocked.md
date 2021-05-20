# `Interlocked`

- [`Interlocked`](#interlocked)
  - [About](#about)
  - [Methods](#methods)
    - [`Add`(`Int32`, `Int32`)](#addint32-int32)
    - [Add(Int64, Int64)](#addint64-int64)

## About

The `System.Threading.Interlocked` class provides static methods that help protect against errors that can occur when the scheduler switches contexts while a thread is updating a variable that can be accessed by other threads, or when two threads are executing concurrently on separate processors. The members of this class do not throw exceptions.

The `Increment` and `Decrement` methods increment or decrement a variable and store the resulting value in a single operation. On most computers, incrementing a variable is not an atomic operation, requiring the following steps:

1. Load a value from an instance variable into a register.
2. Increment or decrement the value.
3. Store the value in the instance variable.

If you do not use `Increment` and `Decrement`, a thread can be preempted after executing the first two steps. Another thread can then execute all three steps. When the first thread resumes execution, it overwrites the value in the instance variable, and the effect of the increment or decrement performed by the second thread is lost.

The `Add` method atomically adds an integer value to an integer variable and returns the new value of the variable.

The `Exchange` method atomically exchanges the values of the specified variables. The `CompareExchange` method combines two operations: comparing two values and storing a third value in one of the variables, based on the outcome of the comparison. The compare and exchange operations are performed as an atomic operation.

## Methods

### `Add`(`Int32`, `Int32`)

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

### Add(Int64, Int64)

Adds two 64-bit integers and replaces the first integer with the sum, as an atomic operation:

```csharp
Console.WriteLine(Int32.MaxValue); // 2147483647

long location1 = Int32.MaxValue;;
long value = 2;

Interlocked.Add(ref location1, value);
Console.WriteLine($"{location1}, {value}"); // 2147483649, 2
```

[↑ Interlocked.Add Method — Microsoft documentation](https://docs.microsoft.com/en-us/dotnet/api/system.threading.interlocked.add)
