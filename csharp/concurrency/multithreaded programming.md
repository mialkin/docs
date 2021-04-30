# Multithreaded programming

A *thread* is an independent executor. Each process has multiple threads in it, and each of those threads can be doing different things simultaneously.Each thread has its own independent stack but shares the same memory with all the other threads in a process. In some applications, there is one thread that is special. For example, user interface applications have a single special UI thread, and Console applications have a single special main thread.

Every .NET application has a *thread pool*. The thread pool maintains a number of *worker threads* that are waiting to execute whatever work you have for them to do. The thread pool is responsible for determining how many threads are in the thread pool at any time. There are dozens of configuration settings you can play with to modify this behavior, but I recommend that you leave it alone; the thread pool has been carefully tuned to cover the vast majority of real-world scenarios. There is almost no need for you to ever create a new thread yourself. The only time you should ever create a `Thread` instance is if you need an STA thread for COM interop.

A thread is a low-level abstraction. The thread pool is a slightly higher level of abstraction; when code queues work to the thread pool, the thread pool itself will take care of creating a thread if necessary. The abstractions covered in this book are higher still: parallel and dataflow processing queues work to the thread pool as necessary. Code using these higher abstractions is easier to get right than code using low-level abstractions.

For this reason, the `Thread` and `BackgroundWorker` types are not covered at all in this book. They have had their time, and that time is over.<sup>1</sup>

<hr>

<sup>1</sup> Stephen Cleary, Concurrency in C# Cookbook: Asynchronous, Parallel, and Multithreaded Programming, Second edition (O'Reilly Media, 2019).