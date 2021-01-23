# Concurrency

**Concurrency** is doing more than one thing at a time.<sup>1</sup>

> *Concurrency* and *parallelism* are not the same thing. **Concurrency** is a way to build things — it's a composition of independently executing things, called *procesees*, typically functions, but they don't have to be. **Parallelism** is, on the other hand, is *simultaneous* execution of multiple things, possibly related, possibly not.
<br><br>If you think about in general way, then concurrency is about *dealing* with a lot of things at once, and parallelism is about *doing* a lot of things at once — those are related, but actually separate ideas. Concurrency is about *structure*, parallelism is about *execution*. <br><br>Concurrency is about structuring things, so that you can, *maybe*, use parallelism to do a better job, but parallelism is not the goal of concurrency; concurrency's goal is a good structure.
<br><br>In order to make concurrency work, you have to add this idea of *communication*. Concurrency gives you a way to structure program into independent pieces, but then you have to coordinate those pieces. And to make that work you need some form of communication. And Tony Hoare in 1978 wrote a paper called "Communicating sequential processes" which is trully one of the greatest papers in computer science.<sup>2</sup>

There are several kinds of concurrency:

* [Asynchronous programming](asynchronous%20programming.md)
* [Parallel programming](parallel%20programming.md)
* [Reactive programming](reactive%20programming.md)
* [Dataflow programming](dataflow%20programming.md)

Usually, a mixture of techniques is used when writing a concurrent program. Most applications at least use multithreading (via the thread pool) and asynchronous programming. Feel free to mix and match all the various forms of concurrency, using the appropriate tool for each part of the application.

## Asynchronous programming

**Asynchronous programming** is a form of concurrency that uses *futures* or *callbacks* to avoid unnecessary threads.

A **future** (or **promise**) is a type that represents some operation that will complete in the future.

**Asynchronous operation** is some operation that is started that will complete some time later. While the operation is in progress, it doesn't block the original thread; the thread that starts the operation is free to do other work.

Asynchronous programming has two primary benefits. The first benefit is for end-user GUI programs: asynchronous programming enables responsiveness. The second benefit is for server-side programs: asynchronous programming enables scalability.

Both benefits of asynchronous programming derive from the same underlying aspect: asynchronous programming frees up a thread. For GUI programs, asynchronous programming frees up the UI thread; this permits the GUI application to remain responsive to user input. For server applications, asynchronous programming frees up request threads; this permits the server to use its threads to serve more requests.

> "Asynchronous" means not waiting for something to complete (e.g., sending data over the network to another node), and not making any assumptions about how long it is going to take.<sup>3</sup>

Modern asynchronous .NET applications use two keywords: `async` and `await`. The `async` keyword is added to a method declaration, and performs a double purpose:

1. Enables the `await` keyword within that method.
2. Signals the compiler to generate a state machine for that method, similar to how `yield return` works.

An `async` method may return `Task<TResult>` if it returns a value, `Task` if it doesn't return a value, or any other "task-like" type, such as `ValueTask`. In addition, an `async` method may return `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` if it returns multiple values in an enumeration. The task-like types represent futures; they can notify the calling code when the `async` method completes. Older asynchronous APIs use callbacks or events instead of futures. When the operation completes, it notifies its future or invokes its callback or event to let the application know the operation is finished.

### Synchronization context

Let's take a quick look at an example:

```csharp
async Task DoSomethingAsync()
{
    int value = 13;
    // Asynchronously wait 1 second.
    await Task.Delay(TimeSpan.FromSeconds(1));
    
    value *= 2;
    
    // Asynchronously wait 1 second.
    await Task.Delay(TimeSpan.FromSeconds(1));
    
    Trace.WriteLine(value);
}
```

An `async` method begins executing synchronously, just like any other method. Within an `async` method, the `await` keyword performs an `asynchronous wait` on its argument. First, it checks whether the operation is already complete; if it is, it continues executing (synchronously). Otherwise, it will pause the `async` method and return an incomplete task. When that operation completes some time later, the `async` method will resume executing.

You can think of an `async` method as having several synchronous portions, broken up by `await` statements. The first synchronous portion executes on whatever thread calls the method, but where do the other synchronous portions execute? The answer is a bit complicated.

When you `await` a task (the most common scenario), a *context* is captured when the `await` decides to pause the method. This is the current `SynchronizationContext` unless it’s `null`, in which case the context is the current `TaskScheduler`. The method resumes executing within that captured context. Usually, this context is the UI context (if you’re on the UI thread) or the threadpool context (most other situations). If you have an ASP.NET Classic (pre-Core) application, then the context could also be an ASP.NET request context. ASP.NET Core uses the threadpool context rather than a special request context.

So, in the preceding code, all the synchronous portions will attempt to resume on the original context. If you call `DoSomethingAsync` from a UI thread, each of its synchronous portions will run on that UI thread; but if you call it from a threadpool thread, each of its synchronous portions will run on any threadpool thread.

You can avoid this default behavior by awaiting the result of the `ConfigureAwait` extension method and passing `false` for the `continueOnCapturedContext` parameter. The following code will start on the calling thread, and after it is paused by an `await`, it’ll resume on a threadpool thread:

```csharp
async Task DoSomethingAsync()
{
    int value = 13;
    
    // Asynchronously wait 1 second.
    await Task.Delay(TimeSpan.FromSeconds(1)).ConfigureAwait(false);
    
    value *= 2;
    
    // Asynchronously wait 1 second.
    await Task.Delay(TimeSpan.FromSeconds(1)).ConfigureAwait(false);
    
    Trace.WriteLine(value);
}
```

## Parallel programming

**Parallel programming** or **parallel processing** is doing lots of work by dividing it up among multiple threads that run concurrently.

Parallel processing uses *multithreading* to maximize the use of multiple processor cores. Parallel processing splits the work among multiple threads, which can each run independently on a different core.

**Multithreading** is a form of concurrency that uses multiple threads of execution.

> Multithreading refers to literally using multiple threads. As demonstrated in many recipes in this book, multithreading is *one* form of concurrency, but certainly not the only one. In fact, direct use of low-level threading types has almost no purpose in a modern application; higher-level abstractions are more powerful and more efficient than old-school multithreading. For that reason, I'll minimize my coverage of outdated techniques. None of the multithreading recipes in this book use the
`Thread` or `BackgroundWorker` types; they have been replaced with superior alternatives.<sup>4</sup>

> **WARNING:** As soon as you type `new Thread()`, it's over; your project already has legacy code.<sup>5</sup>

> But don't get the idea that multithreading is dead! Multithreading lives on in the *thread pool*, a useful place to queue work that automatically adjusts itself according to demand. In turn, the thread pool enables another important form of concurrency: *parallel processing*.<sup>6</sup>

Parallel processing is one type of multithreading, and multithreading is one type of concurrency.

## Reactive programming

Another form of concurrency is *reactive programming*. Asynchronous programming implies that the application will start an operation that will complete once at a later time. Reactive programming is closely related to asynchronous programming but is built on *asynchronous events* instead of *asynchronous operations*. Asynchronous events may not have an actual "start", may happen at any time, and may be raised multiple times. One example is user input.

**Reactive programming** is a declarative style of programming where the application reacts to events.

> Reactive programming isn't necessarily concurrent, but it is closely related to concurrency, so this book covers the basics.<sup>7</sup>

Usually, a mixture of techniques is used when writing a concurrent program. Most applications at least use multithreading (via the thread pool) and asynchronous programming.

<hr>

<sup>1, 4, 5, 6, 7</sup> Stephen Cleary, Concurrency in C# Cookbook: Asynchronous, Parallel, and Multithreaded Programming, Second edition (O'Reilly Media, 2019).

<sup>2</sup> Rob Pike, Concurrency Is Not Parallelism, [↑ talk at Heroku conference](https://vimeo.com/49718712), 2003.

<sup>3</sup> Designing Data-Intensive Applications by Martin Kleppmann (2017), p. 553.
