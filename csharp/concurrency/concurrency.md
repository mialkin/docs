# Concurrency

**Concurrency** is doing more than one thing at a time.<sup>1</sup>

There are several kinds of concurrency:

* parallel
* asynchronous
* reactive
* dataflow

## Mutlithreading

**Multithreading** is a form of concurrency that uses multiple threads of execution.

> Multithreading refers to literally using multiple threads. As demonstrated in many recipes in this book, multithreading is *one* form of concurrency, but certainly not the only one. In fact, direct use of low-level threading types has almost no purpose in a modern application; higher-level abstractions are more powerful and more efficient than old-school multithreading. For that reason, I’ll minimize my coverage of outdated
techniques. None of the multithreading recipes in this book use the
`Thread` or `BackgroundWorker` types; they have been replaced with superior alternatives.<sup>2</sup>

> **WARNING:** As soon as you type `new Thread()`, it’s over; your project already has legacy code.<sup>3</sup>

> But don’t get the idea that multithreading is dead! Multithreading lives on in the *thread pool*, a useful place to queue work that automatically adjusts itself according to demand. In turn, the thread pool enables another important form of concurrency: *parallel processing*.<sup>4</sup>

## Parallel processing

**Parallel processing** is doing lots of work by dividing it up among multiple threads that run concurrently.


<hr>

<sup>1,2,3,4</sup> Stephen Cleary, Concurrency in C# Cookbook: Asynchronous, Parallel, and Multithreaded Programming, Second edition (O’Reilly Media, 2019).
