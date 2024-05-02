# Thread, thread pool

A **thread** is an independent executor, a basic unit to which an operating system allocates processor time.

Each *process* has multiple threads in it, and each of those threads can be doing different things simultaneously.

A **process** is an executing program. An operating system uses processes to separate the applications that are being executed.

Each thread has its own independent stack but shares the same memory with all the other threads in a process.

In some applications, there is one thread that is special. For example, user interface applications have a single special UI thread, and Console applications have a single special main thread.

By default, a .NET program is started with a single thread, often called the *primary thread*. However, it can create additional threads to execute code in parallel or concurrently with the primary thread. These threads are often called *worker threads*.

```csharp
var thread = new Thread(DoWork);
thread.Start();

Console.WriteLine("Start main thread work. " + Environment.CurrentManagedThreadId);
thread.Join();
Console.WriteLine("End main thread work. " + Environment.CurrentManagedThreadId);

void DoWork()
{
    Console.WriteLine("Start background thread work. " + Environment.CurrentManagedThreadId);
    Thread.Sleep(3000);
    Console.WriteLine("End background thread work. " + Environment.CurrentManagedThreadId);
}

// Output:
// Start main thread work. 1
// Start background thread work. 4
// End background thread work. 4
// End main thread work. 1
```

## Table of contents

- [Thread, thread pool](#thread-thread-pool)
  - [Table of contents](#table-of-contents)
  - [Thread context](#thread-context)
  - [Thread pool](#thread-pool)
  - [Thread preemption](#thread-preemption)
  - [References](#references)

## Thread context

Each thread has a [↑ scheduling priority](https://learn.microsoft.com/en-us/dotnet/standard/threading/scheduling-threads) and maintains a set of structures the system uses to save the *thread context* when the thread's execution is paused. The thread context includes all the information the thread needs to seamlessly resume execution, including the thread's set of CPU registers and stack.

Multiple threads can run in the context of a process. All threads of a process share its *virtual address space*. A thread can execute any part of the program code, including parts currently being executed by another thread.

## Thread pool

Every .NET application has a *thread pool*. The thread pool maintains a number of *worker threads* that are waiting to execute whatever work you have for them to do. The thread pool is responsible for determining how many threads are in the thread pool at any time. There are dozens of configuration settings you can play with to modify this behavior, but I recommend that you leave it alone; the thread pool has been carefully tuned to cover the vast majority of real-world scenarios. There is almost no need for you to ever create a new thread yourself. The only time you should ever create a `Thread` instance is if you need an STA thread for COM interop.

A thread is a low-level abstraction. The thread pool is a slightly higher level of abstraction; when code queues work to the thread pool, the thread pool itself will take care of creating a thread if necessary. The abstractions covered in this book are higher still: parallel and dataflow processing queues work to the thread pool as necessary. Code using these higher abstractions is easier to get right than code using low-level abstractions. For this reason, the `Thread` and `BackgroundWorker` types are not covered at all in this book. They have had their time, and that time is over.<sup>1</sup>

Usually, you don't have to worry about how the work is handled by the thread pool. Data and task parallelism use dynamically adjusting partitioners to divide work among worker threads. The thread pool increases its thread count as necessary. The thread pool has a single work queue, and each threadpool thread also has its own work queue. When a threadpool thread queues additional work, it sends it to its own queue first because the work is usually related to the current work item; this behavior encourages threads to work on their own work, and maximizes cache hits. If another thread doesn't have work to do, it'll steal work from another thread's queue.

As long as your tasks are not extremely short, they should work well with the default settings: *tasks should neither be extremely short, nor extremely long*.

If your tasks are too short, then the overhead of breaking up the data into tasks and scheduling those tasks on the thread pool becomes significant. If your tasks are too long, then the thread pool cannot dynamically adjust its work balancing efficiently. It's difficult to determine how short is too short and how long is too long; it really depends on the problem being solved and the approximate capabilities of the hardware. As a general rule, I try to make my tasks as short as possible without running into performance issues (you'll see your performance suddenly degrade when your tasks are too short). Even better, instead of using tasks directly, use the `Parallel` type or PLINQ. These higher-level forms of parallelism have partitioning built in to handle this automatically for you (and adjust as necessary at runtime).

## Thread preemption

A **thread preemption** refers to the interruption of a currently executing thread by the operating system's scheduler in order to give execution time to another thread.

When a higher-priority thread becomes ready to run, or when a time slice allocated to a thread expires, the scheduler may preempt the currently running thread and switch to executing the higher-priority thread. This process helps in multitasking environments where multiple threads or processes compete for CPU resources.

## References

<sup>1</sup> Stephen Cleary, Concurrency in C# Cookbook: Asynchronous, Parallel, and Multithreaded Programming, Second edition (O'Reilly Media, 2019).
