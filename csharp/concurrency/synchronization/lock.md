# `Monitor`, `lock`, `Mutex`

## Table of contents

- [`Monitor`, `lock`, `Mutex`](#monitor-lock-mutex)
  - [Table of contents](#table-of-contents)
  - [`Monitor`](#monitor)
  - [`lock`](#lock)
  - [Nested locking](#nested-locking)
  - [Choosing the synchronization object](#choosing-the-synchronization-object)
  - [`Monitor` vs `Mutex`](#monitor-vs-mutex)
  - [`Mutex`](#mutex)
  - [When to lock](#when-to-lock)

## `Monitor`

The [↑ `Monitor`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.monitor) class provides a mechanism that synchronizes access to objects.

Use the `Enter` and `Exit` methods of `Monitor` instance to mark the beginning and end of a [critical section](/csharp/concurrency/synchronization/synchronization.md#critical-section).

`Monitor` locks objects, that is, reference types, not value types. While you can pass a value type to `Enter` and `Exit`, it is boxed separately for each call. Since each call creates a separate object, `Enter` never blocks, and the code it is supposedly protecting is not really synchronized. In addition, the object passed to `Exit` is different from the object passed to `Enter`, so `Monitor` throws `SynchronizationLockException` exception with the message "Object synchronization method was called from an unsynchronized block of code."

Use the `Monitor` class to lock objects other than strings, that is, reference types other than `string`. Given the way that strings are *sometimes* interned and *sometimes* not, you could easily end up with *accidentally* shared locks where you didn't intend them.

## `lock`

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

## Nested locking

A thread can repeatedly lock the same object in a nested, *reentrant*, fashion:

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

## `Monitor` vs `Mutex`

A `Monitor` is managed, and more lightweight — but is restricted to your [↑ `AppDomain`](https://learn.microsoft.com/en-us/dotnet/api/system.appdomain).

A [`Mutex`](mutex.md) can be named, and can span processes, allowing some simple IPC scenarios between applications.

For most simple scenarios, `Monitor` via `lock` is fine.

## `Mutex`

The [↑ `Mutex`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.mutex) class is a synchronization primitive that can also be used for interprocess synchronization.

Mutex is a synchronization primitive that grants exclusive access to the shared resource to only one thread. If a thread acquires a mutex, the second thread that wants to acquire that mutex is suspended until the first thread releases the mutex.

The `Mutex` class enforces thread identity, so a mutex can be released only by the thread that acquired it. By contrast, the [`Semaphore`](semaphore.md) class does not enforce thread identity.

The thread that owns a mutex can request the same mutex in repeated calls to `WaitOne` without blocking its execution. However, the thread must call the `ReleaseMutex` method the same number of times to release ownership of the mutex.

Mutexes are of two types: *local mutexes*, which are unnamed, and *named system mutexes*. A local mutex exists only within your process. It can be used by any thread in your process that has a reference to the `Mutex` object that represents the mutex. Each unnamed `Mutex` object represents a separate local mutex.

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

## When to lock

As a basic rule, you need to lock around accessing *any writable shared field*.
