# Synchronization. Context switch, critical section, spinning

A **synchronization** is a coordination of the actions of threads for a predictable outcome.

Synchronization constructs can be divided into four categories:

- **Simple blocking methods**
  - These wait for another thread to finish or for a period of time to elapse. [↑ `Thread.Sleep`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.thread.sleep), [↑ `Thread.Join`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.thread.join), and [↑ `Task.Wait`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.wait) are simple blocking methods
- **Locking constructs**
  - These limit the number of threads that can perform some activity or execute a section of code at a time. Exclusive locking constructs are most common — these allow just one thread in at a time, and allow competing threads to access common data without interfering with each other. The standard exclusive locking constructs are `lock` (`Monitor.Enter`/`Monitor.Exit`), `Mutex`, and `SpinLock`. The nonexclusive locking constructs are `Semaphore`, `SemaphoreSlim`, and the reader/writer locks
- **Signaling constructs**
  - These allow a thread to pause until receiving a notification from another, avoiding the need for inefficient polling. There are two commonly used signaling devices: event wait handles and `Monitor`'s `Wait`/`Pulse` methods. Framework 4.0 introduces the `CountdownEvent` and `Barrier` classes
- **Non-blocking synchronization constructs**
  - These protect access to a common field by calling upon processor primitives. The CLR and C# provide the following non-blocking constructs: `Thread.MemoryBarrier`, `Thread.VolatileRead`, `Thread.VolatileWrite`, the `volatile` keyword, and the `Interlocked` class

## Table of contents

- [Synchronization. Context switch, critical section, spinning](#synchronization-context-switch-critical-section-spinning)
  - [Table of contents](#table-of-contents)
  - [Context switch](#context-switch)
  - [Critical section](#critical-section)
  - [Spinning](#spinning)

## Context switch

A **context switch** is the process of storing the state of a process or thread, so that it can be restored and resume execution at a later point.

## Critical section

A **critical section** is a segment of code which is not reentrant; that is, it does not support concurrent access by multiple threads. Often, a critical section is used to protect shared resources.

## Spinning

A **spinning** or **busy-waiting** is a technique in which a process repeatedly checks to see if a condition is true, such as whether keyboard input or a lock is available.
