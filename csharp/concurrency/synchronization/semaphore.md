# `Semaphore`, `SemaphoreSlim`

A **semaphore** is a variable or abstract data type used to control access to a common resource by multiple processes in a concurrent system such as a multitasking operating system.

A semaphore is simply a variable that is used to solve [critical section](/csharp/concurrency/synchronization/synchronization.md#critical-section) problems and to achieve process synchronization in the multi processing environment. A trivial semaphore is a plain variable that is changed (for example, incremented or decremented, or toggled) depending on programmer-defined conditions.

A useful way to think of a semaphore as used in the real-world system is as a record of how many units of a particular resource are available, coupled with operations to adjust that record safely (i.e., to avoid race conditions) as units are acquired or become free, and, if necessary, wait until a unit of the resource becomes available.

Semaphores which allow an arbitrary resource count are called **counting semaphores**, while semaphores which are restricted to the values 0 and 1 (or locked/unlocked, unavailable/available) are called **binary semaphores** and are used to implement locks.

The semaphore concept was invented by Dutch computer scientist Edsger Dijkstra in 1962 or 1963, when Dijkstra and his team were developing an operating system for the Electrologica X8.

## Table of contents

- [`Semaphore`, `SemaphoreSlim`](#semaphore-semaphoreslim)
  - [Table of contents](#table-of-contents)
  - [`Semaphore`](#semaphore)
    - [Example 1](#example-1)
  - [`SemaphoreSlim`](#semaphoreslim)
    - [Example 1](#example-1-1)
    - [Example 2](#example-2)

## `Semaphore`

The [↑ `Semaphore`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.semaphore) class limits the number of threads that can access a resource or pool of resources concurrently.

Semaphores are of two types: *local semaphores* and *named system semaphores*. If you create a `Semaphore` object using a constructor that accepts a name, it is associated with an operating-system semaphore of that name. Named system semaphores are visible throughout the operating system, and can be used to synchronize the activities of processes. You can create multiple `Semaphore` objects that represent the same named system semaphore, and you can use the `OpenExisting` method to open an existing named system semaphore.

A local semaphore exists only within your process. It can be used by any thread in your process that has a reference to the local `Semaphore` object. Each `Semaphore` object is a separate local semaphore.

Unlike [`SemaphoreSlim`](#semaphoreslim) class, `Semaphore` doesn't have any asynchornous methods like `WaitAsync()`.

### Example 1

```csharp
var semaphore = new Semaphore(initialCount: 0, maximumCount: 1);

DoWork();

void DoWork()
{
    Console.WriteLine("Start");
    semaphore.WaitOne();
    Console.WriteLine("End");
}

// Output:
// Start
```

To print `"End"` you have to either set `initialCount` to `1` or call `Semaphore.Release()` before calling `semaphore.WaitOne()`.

## `SemaphoreSlim`

The [↑ `SemaphoreSlim`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.semaphoreslim) class represents a lightweight alternative to [`Semaphore`](#semaphore) that limits the number of threads that can access a resource or pool of resources concurrently.

Unlike the `Semaphore` class, the `SemaphoreSlim` class doesn't support named system semaphores. You can use it as a local semaphore only. The `SemaphoreSlim` class is the recommended semaphore for synchronization within a single app.

### Example 1

```csharp
var semaphore = new SemaphoreSlim(0);

await DoWorkAsync();

async Task DoWorkAsync()
{
    Console.WriteLine("Start");
    await semaphore.WaitAsync();
    Console.WriteLine("End");
}

// Output:
// Start
```

To print `"End"` you need to pass `1` into constructor.

### Example 2

Let only 3 tasks at a time to run the body of `DoWorkAsync`:

```csharp
var semaphore = new SemaphoreSlim(3);

var tasks = new List<Task>();

for (var i = 0; i < 6; i++)
{
    tasks.Add(DoWorkAsync());
}

await Task.WhenAll(tasks);

async Task DoWorkAsync()
{
    try
    {
        await semaphore.WaitAsync();
        Console.WriteLine("Start");
        await Task.Delay(5000);
        Console.WriteLine("End");
    }
    finally
    {
        semaphore.Release();
    }
}

// Output:
// Start
// Start
// Start
// End
// End
// End
// Start
// Start
// Start
// End
// End
// End
```
