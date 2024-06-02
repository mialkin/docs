# Thread, process, `Thread`

## Table of contents

- [Thread, process, `Thread`](#thread-process-thread)
  - [Table of contents](#table-of-contents)
  - [Thread, process, thread preemption, time-slicing](#thread-process-thread-preemption-time-slicing)
  - [`Thread`](#thread)
    - [`Thread.Sleep`](#threadsleep)
    - [`Thread.Join`](#threadjoin)
    - [`Thread.Yield`](#threadyield)

## Thread, process, thread preemption, time-slicing

A **thread** is an independent executor, a basic unit to which an operating system allocates processor time.

Each thread has its own independent stack but shares the same memory with all the other threads in a *process*.

A **process** is an executing program. Each process has multiple threads in it, and each of those threads can be doing different things simultaneously.

A **thread preemption** refers to the interruption of a currently executing thread by the operating system's scheduler in order to give execution time to another thread.

A **time-slicing** is rapid switching of execution between active threads done by a thread scheduler on a single-processor computer.

Under Windows, a time-slice is typically in the tens-of-milliseconds region — much larger than the CPU overhead in actually switching context between one thread and another, which is typically in the few-microseconds region.

In some applications, there is one thread that is special. For example, user interface applications have a single special UI thread, and console applications have a single special main thread.

## `Thread`

The [↑ `Thread`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.thread) class creates and controls a thread, sets its priority, and gets its status.

### `Thread.Sleep`

The [↑ `Thread.Sleep`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.thread.sleep) method suspends the current thread for the specified amount of time.

A thread, while blocked, doesn't consume CPU resources. While waiting on a `Thread.Sleep` or [`Thread.Join`](#threadjoin), a thread is blocked and so does not consume CPU resources.

`Thread.Sleep(0)` relinquishes the thread’s current time slice immediately, voluntarily handing over the CPU to other threads. `Thread.Yield()` method does the same thing—except that it relinquishes only to threads running on the same processor.

### `Thread.Join`

The [↑ `Thread.Join`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.thread.join) method blocks the calling thread until the thread represented by this instance terminates.

### `Thread.Yield`

The [↑ `Thread.Yield`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.thread.yield) method causes the calling thread to yield execution to another thread that is ready to run on the current processor. The operating system selects the thread to yield to.

`Sleep(0)` or `Yield` is occasionally useful in production code for advanced performance tweaks. It's also an excellent diagnostic tool for helping to uncover thread safety issues: if inserting `Thread.Yield()` anywhere in your code makes or breaks the program, you almost certainly have a bug.
