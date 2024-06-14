# `Monitor`, `lock`, `Mutex`, `Semaphore`, `SemaphoreSlim`, `ReaderWriterLockSlim`

## Table of contents

- [`Monitor`, `lock`, `Mutex`, `Semaphore`, `SemaphoreSlim`, `ReaderWriterLockSlim`](#monitor-lock-mutex-semaphore-semaphoreslim-readerwriterlockslim)
  - [Table of contents](#table-of-contents)
  - [When to lock](#when-to-lock)
  - [Comparison of locking constructs](#comparison-of-locking-constructs)
  - [Choosing the synchronization object](#choosing-the-synchronization-object)
  - [`Monitor`](#monitor)
    - [`lock`](#lock)
    - [Nested locking](#nested-locking)
  - [`Mutex`](#mutex)
  - [`Monitor` vs `Mutex`](#monitor-vs-mutex)
  - [Semaphore](#semaphore)
    - [`Semaphore`](#semaphore-1)
    - [`SemaphoreSlim`](#semaphoreslim)
  - [`ReaderWriterLockSlim`](#readerwriterlockslim)

## When to lock

As a basic rule, you need to lock around accessing _any writable shared field_.

## Comparison of locking constructs

Time taken to lock and unlock the construct once on the same thread (assuming no blocking), as measured on an [↑ Intel Core i7 860](https://www.intel.com/content/www/us/en/products/sku/41316/intel-core-i7860-processor-8m-cache-2-80-ghz/specifications.html):

| Construct                                 | Purpose                                                                              | Cross-process | Overhead |
| ----------------------------------------- | ------------------------------------------------------------------------------------ | ------------- | -------- |
| `lock` (`Monitor.Enter` / `Monitor.Exit`) | Ensures just one thread can access a resource (or section of code) at a time         | —             | 20 ns    |
| `ReaderWriterLockSlim`                    | Allows multiple readers to coexist with a single writer                              | —             | 40 ns    |
| `SemaphoreSlim`                           | Ensures not more than a specified number of concurrent threads can access a resource | —             | 200 ns   |
| `Mutex`                                   | Ensures just one thread can access a resource (or section of code) at a time         | Yes           | 1000 ns  |
| `Semaphore`                               | Ensures not more than a specified number of concurrent threads can access a resource | Yes           | 1000 ns  |

[↑ BenchmarkDotNet](https://github.com/dotnet/BenchmarkDotNet) results:

```console
| Method               | Mean      | Error    | StdDev   | Completed Work Items | Lock Contentions | Allocated |
|--------------------- |----------:|---------:|---------:|---------------------:|-----------------:|----------:|
| Lock                 |  15.73 ns | 0.159 ns | 0.149 ns |                    - |                - |         - |
| Monitor              |  16.51 ns | 0.358 ns | 0.794 ns |                    - |                - |         - |
| ReaderWriterLockSlim |  22.91 ns | 0.193 ns | 0.180 ns |                    - |                - |         - |
| SemaphoreSlim        |  54.10 ns | 0.607 ns | 0.538 ns |                    - |                - |         - |
| Mutex                | 450.48 ns | 7.848 ns | 6.957 ns |                    - |                - |         - |
```

```csharp
[ThreadingDiagnoser]
[MemoryDiagnoser]
[Orderer(SummaryOrderPolicy.FastestToSlowest)]
public class Benchmark
{
    private readonly object _locker1 = new();
    private readonly object _locker2 = new();
    private readonly Mutex _mutex = new();
    private readonly SemaphoreSlim _semaphoreSlim = new(1);
    private readonly ReaderWriterLockSlim _readerWriterLockSlim = new();

    [Benchmark]
    public void Lock()
    {
        lock (_locker1)
        {
        }
    }

    [Benchmark]
    public void Monitor()
    {
        var lockTaken = false;
        try
        {
            System.Threading.Monitor.Enter(_locker2, ref lockTaken);
        }
        finally
        {
            if (lockTaken)
            {
                System.Threading.Monitor.Exit(_locker2);
            }
        }
    }

    [Benchmark]
    public void Mutex()
    {
        try
        {
            _mutex.WaitOne();
        }
        finally
        {
            _mutex.ReleaseMutex();
        }
    }

    [Benchmark]
    public async ValueTask SemaphoreSlim()
    {
        try
        {
            await _semaphoreSlim.WaitAsync();
        }
        finally
        {
            _semaphoreSlim.Release();
        }
    }

    [Benchmark]
    public void ReaderWriterLockSlim()
    {
        try
        {
            _readerWriterLockSlim.EnterWriteLock();
        }
        finally
        {
            _readerWriterLockSlim.ExitWriteLock();
        }
    }
}
```


## Choosing the synchronization object

Any object visible to each of the partaking threads can be used as a synchronizing object, subject to one hard rule: it must be a reference type. The synchronizing object is typically private (because this helps to encapsulate the locking logic) and is typically an instance or static field. The synchronizing object can double as the object it's protecting, as the `_list` field does in the following example:

```csharp
class ThreadSafe
{
    private readonly List<string> _list = [];

    void AddItem()
    {
        lock (_list)
        {
            _list.Add("Item 1");
        }
    }
}
```

A field dedicated for the purpose of locking (such as `locker`, in the example prior) allows precise control over the scope and granularity of the lock. The containing object (`this`) — or even its type — can also be used as a synchronization object:

```csharp
lock (this) { ... }
```

```csharp
lock (typeof(Widget)) { ... } // For protecting access to statics
```

The disadvantage of locking in this way is that you're not encapsulating the locking logic, so it becomes harder to prevent deadlocking and excessive blocking. A lock on a type may also seep through application domain boundaries (within the same process).

You can also lock on local variables captured by lambda expressions or anonymous methods.

Locking doesn't restrict access to the synchronizing object itself in any way. In other words, `x.ToString()` will not block because another thread has called `lock(x)`; both threads must call `lock(x)` in order for blocking to occur.

## `Monitor`

The [↑ `Monitor`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.monitor) class provides a mechanism that synchronizes access to objects.

Use the `Enter` and `Exit` methods of `Monitor` instance to mark the beginning and end of a [critical section](/csharp/concurrency/synchronization/synchronization.md#critical-section).

`Monitor` locks objects, that is, reference types, not value types. While you can pass a value type to `Enter` and `Exit`, it is boxed separately for each call. Since each call creates a separate object, `Enter` never blocks, and the code it is supposedly protecting is not really synchronized. In addition, the object passed to `Exit` is different from the object passed to `Enter`, so `Monitor` throws `SynchronizationLockException` exception with the message "Object synchronization method was called from an unsynchronized block of code."

Use the `Monitor` class to lock objects other than strings, that is, reference types other than `string`. Given the way that strings are _sometimes_ interned and _sometimes_ not, you could easily end up with _accidentally_ shared locks where you didn't intend them.

### `lock`

The [↑ `lock`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/lock) statement acquires the mutual-exclusion lock for a given object, executes a statement block, and then releases the lock.

The functionality provided by the `Monitor`'s `Enter` and `Exit` methods is identical to that provided by the `lock` statement, except that the language constructs wrap the `Monitor.Enter(Object, Boolean)` method overload and the `Monitor.Exit` method in a `try/finally` block to ensure that the monitor is released.

```csharp
var locker = new object();

lock (locker)
{
    Console.WriteLine("Hello, world!");
}
```

```csharp
object obj = new object();
bool lockTaken = false;
try
{
    Monitor.Enter(obj, ref lockTaken);
    Console.WriteLine("Hello, world!");
}
finally
{
    if (lockTaken)
      Monitor.Exit(obj);
}
```

Just as with the [Mutex](#mutex), a `lock` statement can be released only from the same thread that obtained it.

### Nested locking

A thread can repeatedly lock the same object in a nested, _reentrant_, fashion:

```csharp
var locker = new object();

lock (locker)
{
    lock (locker)
    {
        lock (locker)
        {
            // Do some work...
        }
    }
}
```

or:

```csharp
var locker = new object();

Monitor.Enter(locker);
Monitor.Enter(locker);
Monitor.Enter(locker);

// Do some work...

Monitor.Exit(locker);
Monitor.Exit(locker);
Monitor.Exit(locker);

```

In these scenarios, the object is unlocked only when the outermost `lock` statement has exited — or a matching number of `Monitor.Exit` statements have executed.

## `Mutex`

The [↑ `Mutex`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.mutex) class is a synchronization primitive that can also be used for interprocess synchronization.

Mutex is a synchronization primitive that grants exclusive access to the shared resource to only one thread. If a thread acquires a mutex, the second thread that wants to acquire that mutex is suspended until the first thread releases the mutex.

The `Mutex` class enforces thread identity, so a mutex can be released only by the thread that acquired it. By contrast, the [`Semaphore`](#semaphore) class does not enforce thread identity.

The thread that owns a mutex can request the same mutex in repeated calls to `WaitOne` without blocking its execution. However, the thread must call the `ReleaseMutex` method the same number of times to release ownership of the mutex.

Mutexes are of two types: _local mutexes_, which are unnamed, and _named system mutexes_. A local mutex exists only within your process. It can be used by any thread in your process that has a reference to the `Mutex` object that represents the mutex. Each unnamed `Mutex` object represents a separate local mutex.

Named system mutexes are visible throughout the operating system, and can be used to synchronize the activities of processes. You can create a `Mutex` object that represents a named system mutex by using a constructor that accepts a name. The operating-system object can be created at the same time, or it can exist before the creation of the `Mutex` object. You can create multiple `Mutex` objects that represent the same named system mutex, and you can use the `OpenExisting` method to open an existing named system mutex.

```csharp
var mutex = new Mutex();

var thread1 = new Thread(DoWork);
var thread2 = new Thread(DoWork);

thread1.Start();
thread2.Start();

void DoWork()
{
    try
    {
        mutex.WaitOne();
        Console.WriteLine("Entering critical section. " + Environment.CurrentManagedThreadId);
        Thread.Sleep(3000);
        Console.WriteLine("Leaving critical section. " + Environment.CurrentManagedThreadId);
    }
    finally
    {
        mutex.ReleaseMutex();
    }
}

// Output:
// Entering critical section. 4
// Leaving critical section. 4
// Entering critical section. 5
// Leaving critical section. 5
```

A common use for a cross-process `Mutex` is to ensure that only one instance of a program can run at a time.

## `Monitor` vs `Mutex`

A `Monitor` is managed, and more lightweight — but is restricted to your [↑ `AppDomain`](https://learn.microsoft.com/en-us/dotnet/api/system.appdomain).

A [`Mutex`](mutex.md) can be named, and can span processes, allowing some simple IPC scenarios between applications.

For most simple scenarios, `Monitor` via `lock` is fine.

## Semaphore

A **semaphore** is a variable or abstract data type used to control access to a common resource by multiple processes in a concurrent system such as a multitasking operating system.

A semaphore is simply a variable that is used to solve [critical section](/csharp/concurrency/synchronization/synchronization.md#critical-section) problems and to achieve process synchronization in the multi-processing environment. A trivial semaphore is a plain variable that is changed, for example, incremented or decremented, or toggled, depending on programmer-defined conditions.

A useful way to think of a semaphore as used in the real-world system is as a record of how many units of a particular resource are available, coupled with operations to adjust that record safely, i.e., to avoid race conditions, as units are acquired or become free, and, if necessary, wait until a unit of the resource becomes available.

Semaphores which allow an arbitrary resource count are called **counting semaphores**, while semaphores which are restricted to the values 0 and 1, or locked/unlocked, unavailable/available, are called **binary semaphores** and are used to implement locks.

The semaphore concept was invented by Dutch computer scientist Edsger Dijkstra in 1962 or 1963, when Dijkstra and his team were developing an operating system for the Electrologica X8.

### `Semaphore`

The [↑ `Semaphore`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.semaphore) class limits the number of threads that can access a resource or pool of resources concurrently.

A semaphore with a capacity of one is similar to a [`Mutex`](locking.md#mutex) or [`lock`](locking.md#lock), except that the semaphore has no "owner" — it's thread-agnostic. Any thread can call `Release` on a `Semaphore`, whereas with `Mutex` and `lock`, only the thread that obtained the lock can release it.

Semaphores are of two types: _local semaphores_ and _named system semaphores_. If you create a `Semaphore` object using a constructor that accepts a name, it is associated with an operating-system semaphore of that name. Named system semaphores are visible throughout the operating system, and can be used to synchronize the activities of processes. You can create multiple `Semaphore` objects that represent the same named system semaphore, and you can use the `OpenExisting` method to open an existing named system semaphore.

A local semaphore exists only within your process. It can be used by any thread in your process that has a reference to the local `Semaphore` object. Each `Semaphore` object is a separate local semaphore.

Unlike [`SemaphoreSlim`](#semaphoreslim) class, `Semaphore` doesn't have any asynchornous methods like `WaitAsync()`.

**Example 1**:

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

To print `"End"` you have to either set `initialCount` to `1` or call `semaphore.Release()` before calling `semaphore.WaitOne()`.

### `SemaphoreSlim`

The [↑ `SemaphoreSlim`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.semaphoreslim) class represents a lightweight alternative to [`Semaphore`](#semaphore) that limits the number of threads that can access a resource or pool of resources concurrently.

Unlike the `Semaphore` class, the `SemaphoreSlim` class doesn't support named system semaphores. You can use it as a local semaphore only. The `SemaphoreSlim` class is the recommended semaphore for synchronization within a single app.

**Example 1**:

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

To print `"End"` you need to pass `1` into constructor or call `semaphore.Release()` before calling `semaphore.WaitAsync()`.

**Example 2**:

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

## `ReaderWriterLockSlim`

The [↑ `ReaderWriterLockSlim`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.readerwriterlockslim) class represents a lock that is used to manage access to a resource, allowing multiple threads for reading or exclusive access for writing.

The `ReaderWriterLockSlim` class is designed to provide maximum-availability locking in scenarios where there are many readers and just occasional updates. An example of where this could occur is in a business application server, where commonly used data is cached for fast retrieval in static fields. Although protecting instances of types with a simple exclusive lock for all modes of access usually does the trick, it can unreasonably restrict concurrency.

When compared to an ordinary `lock` (`Monitor.Enter/Exit`), `ReaderWriterLockSlim` is twice as slow.

There are two basic kinds of lock — a read lock and a write lock:

- A write lock is universally exclusive
- A read lock is compatible with other read locks

A thread holding a write lock blocks all other threads trying to obtain a read or write lock. But if no thread holds a write lock, any number of threads may concurrently obtain a read lock.

`ReaderWriterLockSlim` allows more concurrent Read activity than a simple lock.

```csharp
var readerWriterLockSlim = new ReaderWriterLockSlim();
var items = new List<int>();

new Thread(Read).Start();
new Thread(Read).Start();
new Thread(Read).Start();
new Thread(Write).Start();
new Thread(Write).Start();

void Read()
{
    while (true)
    {
        readerWriterLockSlim.EnterReadLock();
        foreach (var unused in items)
        {
            Thread.Sleep(10);
        }

        Console.WriteLine ($"{readerWriterLockSlim.CurrentReadCount} concurrent readers");
        readerWriterLockSlim.ExitReadLock();
    }
}

void Write()
{
    while (true)
    {
        var newNumber = Random.Shared.Next(100);

        readerWriterLockSlim.EnterWriteLock();
        items.Add(newNumber);
        readerWriterLockSlim.ExitWriteLock();

        Console.WriteLine($"Thread {Environment.CurrentManagedThreadId} added {newNumber}");
        Thread.Sleep(100);
    }
}
```

Sometimes it's useful to swap a read lock for a write lock in a single atomic operation. For instance, suppose you want to add an item to a list only if the item wasn't already present. Ideally, you'd want to minimize the time spent holding the (exclusive) write lock, so you might proceed as follows:

1. Obtain a read lock
2. Test if the item is already present in the list, and if so, release the lock and `return`
3. Release the read lock
4. Obtain a write lock
5. Add the item

The problem is that another thread could sneak in and modify the list, e.g., adding the same item, between steps 3 and 4. `ReaderWriterLockSlim` addresses this through a third kind of lock called an _upgradeable lock_. An upgradeable lock is like a read lock except that it can later be promoted to a write lock in an atomic operation. Here's how you use it:

1. Call `EnterUpgradeableReadLock`
2. Perform read-based activities, e.g., test whether the item is already present in the list
3. Call `EnterWriteLock`, this converts the upgradeable lock to a write lock
4. Perform write-based activities, e.g., add the item to the list
5. Call `ExitWriteLock`, this converts the write lock back to an upgradeable lock.
6. Perform any other read-based activities
7. Call `ExitUpgradeableReadLock`

From the caller's perspective, it's rather like nested or recursive locking. Functionally, though, in step 3, `ReaderWriterLockSlim` releases your read lock and obtains a fresh write lock, atomically.

There's another important difference between upgradeable locks and read locks. While an upgradeable lock can coexist with any number of _read_ locks, only one upgradeable lock can itself be taken out at a time. This prevents conversion deadlocks by _serializing_ competing conversions—just as update locks do in SQL Server:

| SQL Server     | `ReaderWriterLockSlim` |
| -------------- | ---------------------- |
| Share lock     | Read lock              |
| Exclusive lock | Write lock             |
| Update lock    | Upgradeable lock       |

We can demonstrate an upgradeable lock by changing the `Write` method in the preceding example such that it adds a number to list only if not already present:

```csharp
var readerWriterLockSlim = new ReaderWriterLockSlim();
var items = new List<int>();

new Thread(Read).Start();
new Thread(Read).Start();
new Thread(Read).Start();
new Thread(Write).Start();
new Thread(Write).Start();

void Read()
{
    while (true)
    {
        readerWriterLockSlim.EnterReadLock();
        foreach (var unused in items)
        {
            Thread.Sleep(10);
        }

        Console.WriteLine($"{readerWriterLockSlim.CurrentReadCount} concurrent readers");
        readerWriterLockSlim.ExitReadLock();
    }
}

void Write()
{
    while (true)
    {
        var newNumber = Random.Shared.Next(100);

        readerWriterLockSlim.EnterUpgradeableReadLock();

        if (!items.Contains(newNumber))
        {
            readerWriterLockSlim.EnterWriteLock();
            items.Add(newNumber);
            readerWriterLockSlim.ExitWriteLock();
            Console.WriteLine($"Thread {Environment.CurrentManagedThreadId} added {newNumber}");
        }

        readerWriterLockSlim.ExitUpgradeableReadLock();
        Thread.Sleep(100);
    }
}
```
