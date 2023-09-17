# `Semaphore` and `SemaphoreSlim`

- [`Semaphore` and `SemaphoreSlim`](#semaphore-and-semaphoreslim)
  - [`Semaphore`](#semaphore)
    - [Remarks](#remarks)
  - [`SemaphoreSlim`](#semaphoreslim)
    - [Remarks](#remarks-1)
    - [Examples](#examples)
      - [Example 1](#example-1)
      - [Example 2](#example-2)
      - [Example 3](#example-3)
  - [Links](#links)

## `Semaphore`

In computer science, a **semaphore** is a variable or abstract data type used to control access to a common resource by multiple processes in a concurrent system such as a multitasking operating system. A semaphore is simply a variable. This variable is used to solve critical section problems and to achieve process synchronization in the multi processing environment. A trivial semaphore is a plain variable that is changed (for example, incremented or decremented, or toggled) depending on programmer-defined conditions.

A useful way to think of a semaphore as used in the real-world system is as a record of how many units of a particular resource are available, coupled with operations to adjust that record safely (i.e., to avoid race conditions) as units are acquired or become free, and, if necessary, wait until a unit of the resource becomes available.

Semaphores which allow an arbitrary resource count are called **counting semaphores**, while semaphores which are restricted to the values 0 and 1 (or locked/unlocked, unavailable/available) are called **binary semaphores** and are used to implement locks.

The semaphore concept was invented by Dutch computer scientist Edsger Dijkstra in 1962 or 1963, when Dijkstra and his team were developing an operating system for the Electrologica X8.

### Remarks

Semaphores are of two types: *local semaphores* and *named system semaphores*. If you create a `Semaphore` object using a constructor that accepts a name, it is associated with an operating-system semaphore of that name. Named system semaphores are visible throughout the operating system, and can be used to synchronize the activities of processes. You can create multiple `Semaphore` objects that represent the same named system semaphore, and you can use the `OpenExisting` method to open an existing named system semaphore.
A local semaphore exists only within your process. It can be used by any thread in your process that has a reference to the local `Semaphore` object. Each `Semaphore` object is a separate local semaphore.

Note:
> Unlike `SemaphoreSlim` class, `Semaphore` doesn't have any asynchornouse methods like `WaitAsync()`.

## `SemaphoreSlim`

Represents a lightweight alternative to `Semaphore` that limits the number of threads that can access a resource or pool of resources concurrently.

### Remarks

Semaphores are of two types: **local semaphores** and **named system semaphores**. Local semaphores are local to an application, system semaphores are visible throughout the operating system and are suitable for inter-process synchronization. The `SemaphoreSlim` is a lightweight alternative to the `Semaphore` class that doesn't use Windows kernel semaphores. Unlike the `Semaphore` class, the `SemaphoreSlim` class doesn't support named system semaphores. You can use it as a local semaphore only. The `SemaphoreSlim` class is the recommended semaphore for synchronization within a single app.

### Examples

#### Example 1

```csharp
SemaphoreSlim gate = new SemaphoreSlim(0);

await DoWork();

async Task DoWork()
{
    Console.WriteLine("Start");
    await gate.WaitAsync();
    Console.WriteLine("Middle");
    Console.WriteLine("End");
}
```

Outputs:

```output
Start
```

You need to pass `1` into `SemaphoreSlim` constructor to print "Middle" and "End".

#### Example 2

```csharp
SemaphoreSlim gate = new SemaphoreSlim(2);

await DoWork();

async Task DoWork()
{
    for (int i = 0; i < 10; i++)
    {
        Console.WriteLine("Start");
        await gate.WaitAsync();
        Console.WriteLine("Middle");
        Console.WriteLine("End");
    }
}
```

Outputs:

```csharp
Start
Middle
End
Start
Middle
End
Start
```

#### Example 3

```csharp
HttpClient client = new();

var maxParallel = 10;
var throttler = new SemaphoreSlim(initialCount: maxParallel);

Stopwatch stopWatch = new();
stopWatch.Start();

List<Task> tasks = new();
for (int i = 0; i < 100; i++)
{
    tasks.Add(DownloadUri());
}

await Task.WhenAll(tasks);

stopWatch.Stop();

TimeSpan ts = stopWatch.Elapsed;
string elapsedTime = $"{ts.Seconds:00}.{ts.Milliseconds}";

Console.WriteLine(elapsedTime); // 02.586

async Task DownloadUri()
{
    await throttler.WaitAsync();
    try
    {
        HttpResponseMessage response = await client.GetAsync("https://google.com");
        response.EnsureSuccessStatusCode();
        string responseBody = await response.Content.ReadAsStringAsync();
    }
    finally
    {
        throttler.Release();
    }
}
```

## Links

- [↑ Semaphore Class](https://docs.microsoft.com/en-us/dotnet/api/system.threading.semaphore)
- [↑ SemaphoreSlim Class](https://docs.microsoft.com/en-us/dotnet/api/system.threading.semaphoreslim)
- [↑ How do I choose between Semaphore and SemaphoreSlim?](https://stackoverflow.com/questions/4154480/how-do-i-choose-between-semaphore-and-semaphoreslim)
