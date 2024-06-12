# Thread, process, `Thread`

## Table of contents

- [Thread, process, `Thread`](#thread-process-thread)
  - [Table of contents](#table-of-contents)
  - [Thread](#thread)
  - [`Thread`](#thread-1)
    - [`Thread.Sleep`](#threadsleep)
    - [`Thread.Join`](#threadjoin)
    - [`Thread.Yield`](#threadyield)
    - [`Thread.Interrupt`](#threadinterrupt)
    - [`Thread.Abort`](#threadabort)
  - [Background and foreground threads](#background-and-foreground-threads)

## Thread

A **thread** is an independent executor, a basic unit to which an operating system allocates processor time.

Each thread has its own independent stack but shares the same memory with all the other threads in a _process_.

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

`Thread.Sleep(0)` relinquishes the thread's current time slice immediately, voluntarily handing over the CPU to other threads. `Thread.Yield()` method does the same thing—except that it relinquishes only to threads running on the same processor.

### `Thread.Join`

The [↑ `Thread.Join`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.thread.join) method blocks the calling thread until the thread represented by this instance terminates.

### `Thread.Yield`

The [↑ `Thread.Yield`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.thread.yield) method causes the calling thread to yield execution to another thread that is ready to run on the current processor. The operating system selects the thread to yield to.

`Sleep(0)` or `Yield` is occasionally useful in production code for advanced performance tweaks. It's also an excellent diagnostic tool for helping to uncover thread safety issues: if inserting `Thread.Yield()` anywhere in your code makes or breaks the program, you almost certainly have a bug.

### `Thread.Interrupt`

The [↑ `Thread.Interrupt`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.thread.interrupt) method interrupts a thread that is in the [↑ `WaitSleepJoin`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.threadstate) thread state.

Calling `Interrupt` on a blocked thread forcibly releases it, throwing a `ThreadInterruptedException`:

```csharp
var thread = new Thread(() =>
{
    try
    {
        Thread.Sleep(5000);
        Console.WriteLine("Start");
    }
    catch (ThreadInterruptedException exception)
    {
        Console.WriteLine("Thread was interrupted from a waiting state");
    }
    
    Console.WriteLine("Continuing thread execution...");
});

thread.Start();
thread.Interrupt();
```

Interrupting a thread does not cause the thread to end, unless the `ThreadInterruptedException` is unhandled.

If `Interrupt` is called on a thread that's not blocked, the thread continues executing until it next blocks, at which point a `ThreadInterruptedException` is thrown.

Nowadays `Interrupt` is unnecessary: if you are writing the code that blocks, you can achieve the same result more safely with a signaling construct — or Framework 4.0's cancellation tokens.

### `Thread.Abort`

The [↑ `Thread.Abort`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.thread.abort) method raises a `ThreadAbortException` in the thread on which it's invoked, to begin the process of terminating the thread. Calling this method usually terminates the thread.

The `Thread.Abort` APIs are [↑ obsolete](https://learn.microsoft.com/en-us/dotnet/core/compatibility/core-libraries/5.0/thread-abort-obsolete). Starting in .NET 5, calling this method produces compiler warning `SYSLIB0006`. 
Use a `CancellationToken` to abort processing of a unit of work instead of calling `Thread.Abort`.

## Background and foreground threads

By default, threads you create explicitly are _foreground threads_. Foreground threads keep the application alive for as long as any one of them is running, whereas _background threads_ do not. Once all foreground threads finish, the application ends, and any background threads still running abruptly terminate.

```csharp
var worker = new Thread(() => Console.ReadLine())
{
    // IsBackground = true
};

worker.Start();
```

A thread's foreground/background status has no relation to its priority or allocation of execution time.
