# `Interlocked`, `Volatile`, `volatile`

The need for synchronization arises even in the simple case of assigning or incrementing a field. Although locking can always satisfy this need, a contended lock means that a thread must block, suffering the overhead of a context switch and the latency of being descheduled, which can be undesirable in highly concurrent and performance-critical scenarios. The .NET Framework's *nonblocking* synchronization constructs can perform simple operations without ever blocking, pausing, or waiting.

Writing nonblocking or lock-free multithreaded code properly is tricky! Memory barriers, in particular, are easy to get wrong, the [`volatile`](#volatile) keyword is even easier to get wrong. Think carefully whether you really need the performance benefits before dismissing ordinary locks. Remember that acquiring and releasing an uncontended lock takes as little as 20 ns on a 2010-era desktop.

The nonblocking approaches also work across multiple processes. An example of where this might be useful is in reading and writing process-shared memory.

## Table of contents

- [`Interlocked`, `Volatile`, `volatile`](#interlocked-volatile-volatile)
  - [Table of contents](#table-of-contents)
  - [`Interlocked`](#interlocked)
    - [`Add(Int32, Int32)`](#addint32-int32)
    - [`Increment(Int32)`](#incrementint32)
    - [`Decrement(Int32)`](#decrementint32)
    - [`Exchange(Int32, Int32)`](#exchangeint32-int32)
    - [`CompareExchange(Int32, Int32, Int32)`](#compareexchangeint32-int32-int32)
    - [`And(Int32, Int32)`](#andint32-int32)
    - [`Or(Int32, Int32)`](#orint32-int32)
  - [`Thread.MemoryBarrier`](#threadmemorybarrier)
    - [Do we really need locks and barriers?](#do-we-really-need-locks-and-barriers)
  - [`Volatile`](#volatile)
    - [`Volatile.Read`](#volatileread)
    - [`Volatile.Write`](#volatilewrite)
  - [`volatile`](#volatile-1)

## `Interlocked`

The [↑ `System.Threading.Interlocked`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.interlocked) class provides static methods that help protect against errors that can occur when the scheduler switches contexts while a thread is updating a variable that can be accessed by other threads, or when two threads are executing concurrently on separate processors. The members of this class do not throw exceptions.

The [`Increment`](#incrementint32) and [`Decrement`](#decrementint32) methods increment or decrement a variable and store the resulting value in a single operation. On most computers, incrementing a variable is not an atomic operation, requiring the following steps:

1. Load a value from an instance variable into a register.
2. Increment or decrement the value.
3. Store the value in the instance variable.

If you do not use [`Increment`](#incrementint32) and [`Decrement`](#decrementint32), a thread can be [preempted](/csharp/concurrency/thread.md) after executing the first two steps. Another thread can then execute all three steps. When the first thread resumes execution, it overwrites the value in the instance variable, and the effect of the increment or decrement performed by the second thread is lost.

The [`Add`](#addint32-int32) method atomically adds an integer value to an integer variable and returns the new value of the variable.

The [`Exchange`](#exchangeint32-int32) method atomically exchanges the values of the specified variables. The [`CompareExchange`](#compareexchangeint32-int32-int32) method combines two operations: comparing two values and storing a third value in one of the variables, based on the outcome of the comparison. The compare and exchange operations are performed as an atomic operation.

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

## `Thread.MemoryBarrier`

The [↑ `Thread.MemoryBarrier`](https://learn.microsoft.com/ru-ru/dotnet/api/system.threading.thread.memorybarrier) method synchronizes memory access as follows: the processor executing the current thread cannot reorder instructions in such a way that memory accesses prior to the call to `MemoryBarrier()` execute after memory accesses that follow the call to `MemoryBarrier()`.

For most purposes, the `lock` statement or the `Monitor` class provide easier ways to synchronize data.

### Do we really need locks and barriers?

We can demonstrate that memory barriers are important on ordinary Intel Core-2 and Pentium processors with the following short program:

```csharp
var complete = false;

new Thread(() =>
{
    Thread.Sleep(1000);
    complete = true;
}).Start();

Console.WriteLine("Start");

var toggle = false;
while (!complete)
{
    toggle = !toggle;
}

Console.WriteLine("End");

// Output:
// Start
```

You'll need to run it with optimizations enabled and without a debugger:

```bash
dotnet run --configuration=Release
```

This program *never terminates* because the complete variable is cached in a CPU register. Inserting a call to `Thread.MemoryBarrier` inside the while loop, or locking around reading `complete`, fixes the error:

```csharp
while (!complete)
{
    Thread.MemoryBarrier();
    toggle = !toggle;
}
```

```csharp
while (!Volatile.Read(ref complete))
{
    toggle = !toggle;
}
```

```csharp
var wrapper = new Wrapper();
wrapper.Go();

class Wrapper
{
    private bool _complete;
    private readonly object _locker = new();

    public void Go()
    {
        new Thread(() =>
        {
            Thread.Sleep(1000);
            _complete = true;
        }).Start();

        Console.WriteLine("Start");

        var toggle = false;
        while (!_complete)
        {
            lock (_locker)
            {
                toggle = !toggle;
            }
        }

        Console.WriteLine("End");
    }
}

// Output:
// Start
// End
```

Another, more advanced, way to solve this problem is to apply the volatile keyword to the `_complete` field:

```csharp
var wrapper = new Wrapper();
wrapper.Go();

class Wrapper
{
    private volatile bool _complete;

    public void Go()
    {
        new Thread(() =>
        {
            Thread.Sleep(1000);
            _complete = true;
        }).Start();

        Console.WriteLine("Start");

        var toggle = false;
        while (!_complete)
        {
            toggle = !toggle;
        }

        Console.WriteLine("End");
    }
}

// Output:
// Start
// End
```

## `Volatile`

The [↑ `Volatile`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.volatile) class contains methods for performing volatile memory operations.

### `Volatile.Read`

The [↑ `Volatile.Read`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.volatile.read) method reads the value of a field.

On systems that require it, inserts a memory barrier that prevents the processor from reordering memory operations as follows: if a read or write appears *after* this method in the code, the processor cannot move it before this method.

### `Volatile.Write`

The [↑ `Volatile.Write`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.volatile.write) method writes a value to a field.

On systems that require it, inserts a memory barrier that prevents the processor from reordering memory operations as follows: if a read or write appears *before* this method in the code, the processor cannot move it after this method.

## `volatile`

The [↑ `volatile`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/volatile) keyword indicates that a field might be modified by multiple threads that are executing at the same time.
