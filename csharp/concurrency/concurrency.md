# Concurrency

**Concurrency** is doing more than one thing at a time.<sup>1</sup>

## Table of contents

- [Concurrency](#concurrency)
  - [Table of contents](#table-of-contents)
  - [Concurrency vs parallelism](#concurrency-vs-parallelism)
  - [Kinds of concurrency](#kinds-of-concurrency)
    - [Asynchronous programming](#asynchronous-programming)
      - [Synchronization context](#synchronization-context)
    - [Dataflow programming](#dataflow-programming)
      - [Dataflow vs System.Reactive](#dataflow-vs-systemreactive)
      - [Dataflow vs actor frameworks](#dataflow-vs-actor-frameworks)
  - [References](#references)

## Concurrency vs parallelism

Concurrency and parallelism are not the same thing.

**Concurrency** is a way to build things — it's a composition of independently executing things, called *procesees*, typically functions, but they don't have to be.

**Parallelism** is, on the other hand, is *simultaneous* execution of multiple things, possibly related, possibly not.

Concurrency is about *dealing* with a lot of things at once, and parallelism is about *doing* a lot of things at once — those are related, but actually separate ideas. Concurrency is about *structure*, parallelism is about *execution*.

Concurrency is about structuring things, so that you can, *maybe*, use parallelism to do a better job, but parallelism is not the goal of concurrency; concurrency's goal is a good structure.

In order to make concurrency work, you have to add this idea of *communication*. Concurrency gives you a way to structure program into independent pieces, but then you have to coordinate those pieces. And to make that work you need some form of communication. Tony Hoare in 1978 wrote a paper called "Communicating sequential processes" which is truly one of the greatest papers in computer science.<sup>2</sup>

## Kinds of concurrency

There are several kinds of concurrency: [asynchronous programming](asynchronous-programming.md), [parallel programming](parallel-programming.md), [reactive programming](reactive-programming.md), [dataflow programming](dataflow-programming.md)

Usually, a mixture of techniques is used when writing a concurrent program. Most applications at least use multithreading, via the thread pool, and asynchronous programming.

### Asynchronous programming

**Asynchronous programming** is a form of concurrency that uses _futures_ or _callbacks_ to avoid unnecessary threads.

A **future** or **promise** is a type that represents some operation that will complete in the future.

**Asynchronous operation** is some operation that is started that will complete some time later. While the operation is in progress, it doesn't block the original thread; the thread that starts the operation is free to do other work.

Asynchronous programming has two primary benefits. The first benefit is for end-user GUI programs: asynchronous programming enables responsiveness. The second benefit is for server-side programs: asynchronous programming enables scalability.

Both benefits of asynchronous programming derive from the same underlying aspect: asynchronous programming frees up a thread. For GUI programs, asynchronous programming frees up the UI thread; this permits the GUI application to remain responsive to user input. For server applications, asynchronous programming frees up request threads; this permits the server to use its threads to serve more requests.

> "Asynchronous" means not waiting for something to complete, e.g., sending data over the network to another node, and not making any assumptions about how long it is going to take.<sup>1</sup>

Modern asynchronous .NET applications use two keywords: `async` and `await`. The `async` keyword is added to a method declaration, and performs a double purpose:

1. Enables the `await` keyword within that method.
2. Signals the compiler to generate a state machine for that method, similar to how `yield return` works.

An `async` method may return `Task<TResult>` if it returns a value, `Task` if it doesn't return a value, or any other "task-like" type, such as `ValueTask`. In addition, an `async` method may return `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` if it returns multiple values in an enumeration. The task-like types represent futures; they can notify the calling code when the `async` method completes. Older asynchronous APIs use callbacks or events instead of futures.

#### Synchronization context

A **synchronization context** is a mechanism that manages the execution context for asynchronous operations. It helps control how asynchronous callbacks are marshaled between threads. The synchronization context ensures that code scheduled to run on a specific context executes within that context, which is important for scenarios involving user interfaces, where updates to the UI must occur on the UI thread.

The `SynchronizationContext` class is a part of the .NET framework and is designed to provide a way to capture and propagate the execution context. It defines methods like `Post` and `Send` that allow you to send delegates for execution on a specific synchronization context.

For example, in UI applications, like those built using WPF or WinForms, there is typically a synchronization context associated with the UI thread. When asynchronous operations complete, and you need to update the UI based on the results, the synchronization context helps ensure that the UI updates are performed on the UI thread. This is crucial because UI elements are not thread-safe, and updating them from a background thread can lead to unpredictable behavior.

In modern C# asynchronous programming, the `async` and `await` keywords handle synchronization context automatically in many cases. However, it's essential to understand synchronization context when dealing with custom or advanced asynchronous scenarios or when working with older asynchronous patterns that don't inherently support the `async/await` model.

### Dataflow programming

TPL Dataflow is capable of handling any kind of _mesh_. You can define forks, joins, and loops in a mesh, and TPL Dataflow will handle them appropriately. Most of the time, though, TPL Dataflow meshes are used as a pipeline.

The basic building unit of a dataflow mesh is a _dataflow block_. A block can either be a _target block_ (receiving data), a _source block_ (producing data), or both. Source blocks can be linked to target blocks to create the mesh. It's possible to break links and create new blocks and add them to the mesh _while_ there is data flowing through it, but that is a very advanced scenario.

Target blocks have buffers for the data they receive. Having buffers enables them to accept new data items even if they aren't ready to process them yet; this keeps data flowing through the mesh.

TPL Dataflow namespace:

```csharp
System.Threading.Tasks.Dataflow
```

#### Dataflow vs System.Reactive

At first glance, dataflow meshes sound very much like observable streams, and they do have much in common. Both meshes and streams have the concept of data items passing through them. Also, both meshes and streams have the notion of a normal completion (a notification that no more data is coming), as well as a faulting completion (a notification that some error occurred during data processing). But System.Reactive (Rx) and TPL Dataflow do not have the same capabilities. Rx observables are generally better than dataflow blocks when doing anything related to timing. Dataflow blocks are generally better than Rx observables when doing parallel processing. Conceptually, Rx works more like setting up callbacks: each step in the observable directly calls the next step. In contrast, each block in a dataflow mesh is very independent from all the other blocks. Both Rx and TPL Dataflow have their own uses, with some amount of overlap. They also work quite well together.

#### Dataflow vs actor frameworks

If you're familiar with actor frameworks, TPL Dataflow will seem to share similarities with them. Each dataflow block is independent, in the sense that it will spin up tasks to do work as needed, like executing a transformation delegate or pushing output to the next block. You can also set up each block to run in parallel, so that it'll spin up multiple tasks to deal with additional input. Due to this behavior, each block does have a certain similarity to an actor in an actor framework. However, TPL Dataflow is not a full actor framework; in particular, there's no built-in support for clean error recovery or retries of any kind. TPL Dataflow is a library with an actor-like feel, but it isn't a full-featured actor framework.

## References

<sup>1</sup> Stephen Cleary, Concurrency in C# Cookbook: Asynchronous, Parallel, and Multithreaded Programming, Second edition (O'Reilly Media, 2019).

<sup>2</sup> Rob Pike, Concurrency Is Not Parallelism, [↑ talk at Heroku conference](https://vimeo.com/49718712), 2003.
