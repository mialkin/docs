# Synchronization

- [Synchronization](#synchronization)
  - [`lock` statement](#lock-statement)
  - [Synchronization constructs](#synchronization-constructs)
    - [Simple blocking methods](#simple-blocking-methods)
    - [Locking constructs](#locking-constructs)
    - [Signaling constructs](#signaling-constructs)
    - [Nonblocking synchronization constructs](#nonblocking-synchronization-constructs)

When your application makes use of concurrency (as practically all .NET applications do), then you need to watch out for situations in which one piece of code needs to update data while other code needs to access the same data. Whenever this happens, you need to _synchronize_ access to the
data.

There are two major types of synchronization: _communication_ and _data protection_.

You need to use synchronization to protect shared data when all three of these conditions are true:

- Multiple pieces of code are running concurrently.
- These pieces are accessing (reading or writing) the same data.
- At least one piece of code is updating (writing) the data.

In other words, only data that is both _shared_ and _updated_ needs synchronization.

## `lock` statement

There are many other kinds of locks in the .NET framework except `lock` statement, such as `Monitor`, `SpinLock`, and `ReaderWriterLockSlim`. In most applications, these lock types should almost never be used directly. In particular, it’s natural for developers to jump to `ReaderWriterLockSlim` when there is no need for that level of complexity. The basic lock statement handles 99% of cases quite well.

There are four important guidelines when using locks:

- Restrict lock visibility.
- Document what the lock protects.
- Minimize code under lock.
- Never execute arbitrary code while holding a lock.

## Synchronization constructs

Synchronization constructs can be divided into four categories:

### Simple blocking methods

These wait for another thread to finish or for a period of time to elapse. `Sleep`, `Join`, and `Task.Wait` are simple blocking methods.

### Locking constructs

These limit the number of threads that can perform some activity or execute a section of code at a time. Exclusive locking constructs are most common — these allow just one thread in at a time, and allow competing threads to access common data without interfering with each other. The standard exclusive locking constructs are `lock` (`Monitor.Enter`/`Monitor.Exit`), `Mutex`, and `SpinLock`. The nonexclusive locking constructs are `Semaphore`, `SemaphoreSlim`, and the reader/writer locks.

### Signaling constructs

These allow a thread to pause until receiving a notification from another, avoiding the need for inefficient polling. There are two commonly used signaling devices: event wait handles and `Monitor`'s `Wait`/`Pulse` methods. Framework 4.0 introduces the `CountdownEvent` and `Barrier` classes.

### Nonblocking synchronization constructs

These protect access to a common field by calling upon processor primitives. The CLR and C# provide the following nonblocking constructs: `Thread.MemoryBarrier`, `Thread.VolatileRead`, `Thread.VolatileWrite`, the `volatile` keyword, and the `Interlocked` class.
