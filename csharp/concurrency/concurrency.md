# Concurrency

**Concurrency** is doing more than one thing at a time.<sup>1</sup>

> *Concurrency* and *parallelism* are not the same thing. **Concurrency** is a way to build things — it's a composition of independently executing things, called *procesees*, typically functions, but they don't have to be. **Parallelism** is, on the other hand, is *simultaneous* execution of multiple things, possibly related, possibly not.
<br><br>If you think about in general way, then concurrency is about *dealing* with a lot of things at once, and parallelism is about *doing* a lot of things at once — those are related, but actually separate ideas. Concurrency is about *structure*, parallelism is about *execution*. <br><br>Concurrency is about structuring things, so that you can, *maybe*, use parallelism to do a better job, but parallelism is not the goal of concurrency; concurrency's goal is a good structure.
<br><br>In order to make concurrency work, you have to add this idea of *communication*. Concurrency gives you a way to structure program into independent pieces, but then you have to coordinate those pieces. And to make that work you need some form of communication. And Tony Hoare in 1978 wrote a paper called "Communicating sequential processes" which is trully one of the greatest papers in computer science.<sup>2</sup>

There are several kinds of concurrency:

* Parallel programming
* Asynchronous programming
* Reactive programming
* Dataflow programming

**Multithreading** is a form of concurrency that uses multiple threads of execution.

> Multithreading refers to literally using multiple threads. As demonstrated in many recipes in this book, multithreading is *one* form of concurrency, but certainly not the only one. In fact, direct use of low-level threading types has almost no purpose in a modern application; higher-level abstractions are more powerful and more efficient than old-school multithreading. For that reason, I'll minimize my coverage of outdated
techniques. None of the multithreading recipes in this book use the
`Thread` or `BackgroundWorker` types; they have been replaced with superior alternatives.<sup>3</sup>

> **WARNING:** As soon as you type `new Thread()`, it's over; your project already has legacy code.<sup>4</sup>

> But don't get the idea that multithreading is dead! Multithreading lives on in the *thread pool*, a useful place to queue work that automatically adjusts itself according to demand. In turn, the thread pool enables another important form of concurrency: *parallel processing*.<sup>5</sup>

Usually, a mixture of techniques is used when writing a concurrent program. Most applications at least use multithreading (via the thread pool) and asynchronous programming. Feel free to mix and match all the various forms of concurrency, using the appropriate tool for each part of the application.

## Parallel programming

**Parallel programming** or **parallel processing** is doing lots of work by dividing it up among multiple threads that run concurrently.

Parallel processing uses multithreading to maximize the use of multiple processor cores. Parallel processing splits the work among multiple threads, which can each run independently on a different core.

Parallel processing is one type of multithreading, and multithreading is one type of concurrency.

## Asynchronous programming

> "Asynchronous" means not waiting for something to complete (e.g., sending data over the network to another node), and not making any assumptions about how long it is going to take.<sup>6</sup>

**Asynchronous programming** is a form of concurrency that uses *futures* or callbacks to avoid unnecessary threads.

A **future** (or **promise**) is a type that represents some operation that will complete in the future.

**Asynchronous operation** is some operation that is started that will complete some time later. While the operation is in progress, it doesn't block the original thread; the thread that starts the operation is free to do other work.

Some modern future types in .NET are `Task` and `Task<TResult>`. Older asynchronous APIs use callbacks or events instead of futures. When the operation completes, it notifies its future or invokes its callback or event to let the application know the operation is finished.

## Reactive programming

Another form of concurrency is *reactive programming*. Asynchronous programming implies that the application will start an operation that will complete once at a later time. Reactive programming is closely related to asynchronous programming but is built on *asynchronous events* instead of *asynchronous operations*. Asynchronous events may not have an actual "start", may happen at any time, and may be raised multiple times. One example is user input.

**Reactive programming** is a declarative style of programming where the application reacts to events.

> Reactive programming isn't necessarily concurrent, but it is closely related to concurrency, so this book covers the basics.<sup>7</sup>

Usually, a mixture of techniques is used when writing a concurrent program. Most applications at least use multithreading (via the thread pool) and asynchronous programming.

<hr>

<sup>1, 3, 4, 5, 7</sup> Stephen Cleary, Concurrency in C# Cookbook: Asynchronous, Parallel, and Multithreaded Programming, Second edition (O'Reilly Media, 2019).

<sup>2</sup> Rob Pike, Concurrency Is Not Parallelism, [↑ talk at Heroku conference](https://vimeo.com/49718712), 2003.

<sup>6</sup> Designing Data-Intensive Applications by Martin Kleppmann (2017), p. 553.
