# Reactive programming

Another form of concurrency is *reactive programming*. Asynchronous programming implies that the application will start an operation that will complete once at a later time. Reactive programming is closely related to asynchronous programming but is built on *asynchronous events* instead of *asynchronous operations*. Asynchronous events may not have an actual "start", may happen at any time, and may be raised multiple times. One example is user input.

**Reactive programming** is a declarative style of programming where the application reacts to events.

> Reactive programming isn't necessarily concurrent, but it is closely related to concurrency, so this book covers the basics.<sup>1</sup>

Usually, a mixture of techniques is used when writing a concurrent program. Most applications at least use multithreading (via the thread pool) and asynchronous programming.

<hr>

<sup>1</sup> Stephen Cleary, Concurrency in C# Cookbook: Asynchronous, Parallel, and Multithreaded Programming, Second edition (O'Reilly Media, 2019).
