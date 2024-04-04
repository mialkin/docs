# Concurrency

**Concurrency** is doing more than one thing at a time.<sup>1</sup>

## Table of contents

- [Concurrency](#concurrency)
  - [Table of contents](#table-of-contents)
  - [Kinds of concurrency](#kinds-of-concurrency)
  - [Concurrency vs parallelism](#concurrency-vs-parallelism)
  - [Multithreaded programming](#multithreaded-programming)
  - [References](#references)

## Kinds of concurrency

There are several kinds of concurrency: [asynchronous programming](asynchronous-programming.md), [parallel programming](parallel-programming.md), [reactive programming](reactive-programming.md), [dataflow programming](dataflow-programming.md)

Usually, a mixture of techniques is used when writing a concurrent program. Most applications at least use multithreading, via the thread pool, and asynchronous programming.

## Concurrency vs parallelism

Concurrency and parallelism are not the same thing.

**Concurrency** is a way to build things — it's a composition of independently executing things, called *procesees*, typically functions, but they don't have to be.

**Parallelism** is, on the other hand, is *simultaneous* execution of multiple things, possibly related, possibly not.

Concurrency is about *dealing* with a lot of things at once, and parallelism is about *doing* a lot of things at once — those are related, but actually separate ideas. Concurrency is about *structure*, parallelism is about *execution*.

Concurrency is about structuring things, so that you can, *maybe*, use parallelism to do a better job, but parallelism is not the goal of concurrency; concurrency's goal is a good structure.

In order to make concurrency work, you have to add this idea of *communication*. Concurrency gives you a way to structure program into independent pieces, but then you have to coordinate those pieces. And to make that work you need some form of communication. Tony Hoare in 1978 wrote a paper called "Communicating sequential processes" which is truly one of the greatest papers in computer science.<sup>2</sup>

## Multithreaded programming

A *thread* is an independent executor. Each process has multiple threads in it, and each of those threads can be doing different things simultaneously. Each thread has its own independent stack but shares the same memory with all the other threads in a process. In some applications, there is one thread that is special. For example, user interface applications have a single special UI thread, and Console applications have a single special main thread.

Every .NET application has a *thread pool*. The thread pool maintains a number of *worker threads* that are waiting to execute whatever work you have for them to do. The thread pool is responsible for determining how many threads are in the thread pool at any time. There are dozens of configuration settings you can play with to modify this behavior, but I recommend that you leave it alone; the thread pool has been carefully tuned to cover the vast majority of real-world scenarios. There is almost no need for you to ever create a new thread yourself. The only time you should ever create a `Thread` instance is if you need an STA thread for COM interop.

A thread is a low-level abstraction. The thread pool is a slightly higher level of abstraction; when code queues work to the thread pool, the thread pool itself will take care of creating a thread if necessary. The abstractions covered in this book are higher still: parallel and dataflow processing queues work to the thread pool as necessary. Code using these higher abstractions is easier to get right than code using low-level abstractions.

For this reason, the `Thread` and `BackgroundWorker` types are not covered at all in this book. They have had their time, and that time is over.<sup>3</sup>

## References

<sup>1, 3</sup> Stephen Cleary, Concurrency in C# Cookbook: Asynchronous, Parallel, and Multithreaded Programming, Second edition (O'Reilly Media, 2019).

<sup>2</sup> Rob Pike, Concurrency Is Not Parallelism, [↑ talk at Heroku conference](https://vimeo.com/49718712), 2003.
