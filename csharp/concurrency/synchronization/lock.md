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

| Construct                                 | Cross-process | Overhead |
| ----------------------------------------- | ------------- | -------- |
| `lock` (`Monitor.Enter` / `Monitor.Exit`) | —             | 20 ns    |
| `ReaderWriterLockSlim`                    | —             | 40 ns    |
| `SemaphoreSlim`                           | —             | 200 ns   |
| `Mutex`                                   | Yes           | 1000 ns  |
| `Semaphore`                               | Yes           | 1000 ns  |

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

A semaphore with a capacity of one is similar to a [`Mutex`](lock.md#mutex) or [`lock`](lock.md#lock), except that the semaphore has no "owner" — it's thread-agnostic. Any thread can call `Release` on a `Semaphore`, whereas with `Mutex` and `lock`, only the thread that obtained the lock can release it.

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

Quite often, instances of a type are thread-safe for concurrent read operations, but not for concurrent updates (nor for a concurrent read and update). This can also be true with resources such as a file. Although protecting instances of such types with a simple exclusive lock for all modes of access usually does the trick, it can unreasonably restrict concurrency if there are many readers and just occasional updates. An example of where this could occur is in a business application server, where commonly used data is cached for fast retrieval in static fields. The `ReaderWriterLockSlim` class is designed to provide maximum-availability locking in just this scenario.

`ReaderWriterLockSlim` was introduced in Framework 3.5 and is a replacement for the older "fat" [↑ `ReaderWriterLock`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.readerwriterlock) class. The latter is similar in functionality, but it is several times slower and has an inherent design fault in its mechanism for handling lock upgrades.

When compared to an ordinary lock (`Monitor.Enter`/`Exit`), `ReaderWriterLockSlim` is twice as slow.
