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
  - [`SpinLock`](#spinlock)
  - [`SpinWait`](#spinwait)

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

1. Call [↑ `EnterUpgradeableReadLock`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.readerwriterlockslim.enterupgradeablereadlock)
2. Perform read-based activities, e.g., test whether the item is already present in the list
3. Call [↑ `EnterWriteLock`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.readerwriterlockslim.enterwritelock), this converts the upgradeable lock to a write lock
4. Perform write-based activities, e.g., add the item to the list
5. Call `ExitWriteLock`, this converts the write lock back to an upgradeable lock.
6. Perform any other read-based activities
7. Call `ExitUpgradeableReadLock`

From the caller's perspective, it's rather like nested or recursive locking. Functionally, though, in step 3, `ReaderWriterLockSlim` releases your read lock and obtains a fresh write lock, atomically.

There's another important difference between upgradeable locks and read locks. While an upgradeable lock can coexist with any number of _read_ locks, only one upgradeable lock can itself be taken out at a time. This prevents conversion deadlocks by _serializing_ competing conversions — just as update locks do in SQL Server:

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

Only one thread can enter upgradeable mode at any given time. If a thread is in upgradeable mode, and there are no threads waiting to enter write mode, any number of other threads can enter read mode, even if there are threads waiting to enter upgradeable mode.

```csharp
var lockSlim = new ReaderWriterLockSlim();

new Thread(ReadOrWrite).Start();
new Thread(ReadOrWrite).Start();
new Thread(Read).Start();
new Thread(Read).Start();

Thread.Sleep(6000);
Console.WriteLine("Firing final read thread");
new Thread(Read).Start();

void ReadOrWrite()
{
    lockSlim.EnterUpgradeableReadLock();
    Console.WriteLine($"Thread {Environment.CurrentManagedThreadId} entered upgradable lock");

    Thread.Sleep(5000);

    lockSlim.EnterWriteLock();
    Console.WriteLine($"Thread {Environment.CurrentManagedThreadId} entered write lock");
    Thread.Sleep(2000);
    lockSlim.ExitWriteLock();
    Console.WriteLine($"Thread {Environment.CurrentManagedThreadId} exited write lock");

    lockSlim.ExitUpgradeableReadLock();
    Console.WriteLine($"Thread {Environment.CurrentManagedThreadId} exited upgradable lock");
}

void Read()
{
    lockSlim.EnterReadLock();
    Console.WriteLine($"Thread {Environment.CurrentManagedThreadId} entered read lock");

    Thread.Sleep(1000);

    lockSlim.ExitReadLock();
    Console.WriteLine($"Thread {Environment.CurrentManagedThreadId} exited read lock");
}

// Output:
// Thread 7 entered read lock
// Thread 6 entered read lock
// Thread 4 entered upgradable lock
// Thread 7 exited read lock
// Thread 6 exited read lock
// Thread 4 entered write lock
// Firing final read thread
// Thread 4 exited write lock
// Thread 4 exited upgradable lock
// Thread 8 entered read lock
// Thread 5 entered upgradable lock
// Thread 8 exited read lock
// Thread 5 entered write lock
// Thread 5 exited write lock
// Thread 5 exited upgradable lock
```

## `SpinLock`

The [↑ `SpinLock`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.spinlock) structure provides a mutual exclusion lock primitive where a thread trying to acquire the lock waits in a loop repeatedly checking until the lock becomes available.

The `SpinLock` struct lets you lock without incurring the cost of a [context switch](synchronization.md#context-switch), at the expense of keeping a thread [spinning](synchronization.md#spinning), uselessly busy. This approach is valid in high-contention scenarios when locking will be very brief, e.g., in writing a thread-safe linked list from scratch.

If you leave a spinlock contended for too long, we're talking milliseconds at most, it will yield its time slice, causing a context switch just like an ordinary lock. When rescheduled, it will yield again — in a continual cycle of "spin yielding." This consumes far fewer CPU resources than outright spinning — but more than blocking. On a single-core machine, a spinlock will start "spin yielding" immediately if contended.

```csharp
var spinLock = new SpinLock(enableThreadOwnerTracking: true);

new Thread(Go).Start();
new Thread(Go).Start();

void Go()
{
    bool lockTaken = false;
    try
    {
        Console.WriteLine("Start from thread " + Environment.CurrentManagedThreadId);
        spinLock.Enter(ref lockTaken);
        Thread.Sleep(1);
        Console.WriteLine("End from thread " + Environment.CurrentManagedThreadId);
    }
    finally
    {
        if (lockTaken)
        {
            spinLock.Exit();
        }
    }
}
```

Using a `SpinLock` is like using an ordinary [`lock`](#lock), except:

- `SpinLock` is a structure
- `SpinLock` is not reentrant, meaning that you cannot call `Enter` on the same `SpinLock` twice in a row on the same thread. If you violate this rule, it will either throw an exception, if _owner tracking_ is enabled, or deadlock, if owner tracking is disabled. You can specify whether to enable owner tracking when constructing the `SpinLock` instance. Owner tracking incurs a performance hit.
- `SpinLock` lets you query whether the lock is taken, via the properties [↑ `IsHeld`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.spinlock.isheld) and, if owner tracking is enabled, [↑ `IsHeldByCurrentThread`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.spinlock.isheldbycurrentthread)
- There's no equivalent to C#'s [`lock`](locking.md#lock) statement to provide `SpinLock` syntactic sugar

Another difference is that when you call `Enter`, you _must_ follow the robust pattern of providing a `lockTaken` argument, which is nearly always done within a `try/finally` block.

As with an ordinary `lock`, `lockTaken` will be false after calling `Enter` if, and only if, the `Enter` method throws an exception and the `lock` was not taken. This happens in very rare scenarios, such as [`Abort`](/csharp/concurrency/thread.md#threadabort) being called on the thread, or an `OutOfMemoryException` being thrown, and lets you reliably know whether to subsequently call `Exit`.

`SpinLock` also provides a [↑ `TryEnter`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.spinlock.tryenter) method which accepts a timeout.

A `SpinLock` makes the most sense when writing your own reusable synchronization constructs. Even then, a spinlock is not as useful as it sounds. It still limits concurrency. And it wastes CPU time doing _nothing useful_. Often, a better choice is to spend some of that time doing something _speculative_ — with the help of [`SpinWait`](#spinwait).

## `SpinWait`

The [↑ `SpinWait`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.spinwait) structure provides support for spin-based waiting.

`SpinWait` helps you write lock-free code that spins rather than blocks. It works by implementing safeguards to avoid the dangers of resource starvation and [↑ priority inversion](https://en.wikipedia.org/wiki/Priority_inversion) that might otherwise arise with spinning.

Lock-free programming with `SpinWait` is as _hardcore_ as multithreading gets and is intended for when none of the higher-level constructs will do. A prerequisite is to understand nonblocking synchronization.

Suppose we wrote a spin-based signaling system based purely on a simple flag:

```csharp
var proceed = false;

void Test()
{
    // Spin until another thread sets proceed to true
    while (!proceed) Thread.MemoryBarrier();
}
```

This would be highly efficient if `Test` ran when `proceed` was already true — or if `proceed` became `true` within a few cycles. But now suppose that `proceed` remained false for several seconds — and that four threads called `Test` at once. The spinning would then fully consume a quad-core CPU! This would cause other threads to run slowly, resource starvation, — including the very thread that might eventually set `proceed` to true, [↑ priority inversion](https://en.wikipedia.org/wiki/Priority_inversion). The situation is exacerbated on single-core machines, where spinning will nearly _always_ cause priority inversion. And although single-core machines are rare nowadays, single-core _virtual machines_ are not.

`SpinWait` addresses these problems in two ways. First, it limits CPU-intensive spinning to a set number of iterations, after which it yields its time slice on every spin, by calling [`Thread.Yield`](/csharp/concurrency/thread.md#threadyield) and [`Thread.Sleep`](/csharp/concurrency/thread.md#threadsleep), lowering its resource consumption. Second, it detects whether it's running on a single-core machine, and if so, it yields on every cycle.

In its current implementation, `SpinWait` performs CPU-intensive spinning for 10 iterations before yielding. However, it doesn't return to the caller _immediately_ after each of those cycles: instead, it calls `Thread.SpinWait` to spin via the CLR, and ultimately the operating system, for a set time period. This time period is initially a few tens of nanoseconds, but doubles with each iteration until the 10 iterations are up. This ensures some predictability in the total time spent in the CPU-intensive spinning phase, which the CLR and operating system can tune according to conditions. Typically, it's in the few-tens-of-microseconds region — small, but more than the cost of a context switch.

On a single-core machine, `SpinWait` yields on every iteration. You can test whether `SpinWait` will yield on the next spin via the property `NextSpinWillYield`.

If a `SpinWait` remains in "spin-yielding" mode for long enough, maybe 20 cycles, it will periodically sleep for a few milliseconds to further save resources and help other threads progress.

`SpinWait` is a value type, which means that low-level code can utilize `SpinWait` without fear of unnecessary allocation overheads. `SpinWait` is not generally useful for ordinary applications. In most cases, you should use the synchronization classes provided by the .NET Framework, such as `Monitor`. For most purposes where spin waiting is required, however, the `SpinWait` type should be preferred over the `Thread.SpinWait` method.

```csharp
var numbers = ParallelEnumerable.Range(1, 1_000_000);

long sum = 0;
var spinWait = new SpinWait();

numbers.ForAll(x =>
{
    while (true)
    {
        var snapshot = sum;
        Thread.MemoryBarrier();
        var newSum = snapshot + x;
        if (Interlocked.CompareExchange(ref sum, newSum, snapshot) == snapshot)
        {
            return;
        }

        spinWait.SpinOnce();
    }
});

Console.WriteLine(sum);
// Output:
// 500000500000
```
