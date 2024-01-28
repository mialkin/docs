# Asynchronous programming

- [Asynchronous programming](#asynchronous-programming)
  - [Synchronization context](#synchronization-context)
    - [Example](#example)
  - [Deadlock](#deadlock)
  - [Task.Yield()](#taskyield)
  - [async void](#async-void)

**Asynchronous programming** is a form of concurrency that uses _futures_ or _callbacks_ to avoid unnecessary threads.

A **future** or **promise** is a type that represents some operation that will complete in the future.

**Asynchronous operation** is some operation that is started that will complete some time later. While the operation is in progress, it doesn't block the original thread; the thread that starts the operation is free to do other work.

Asynchronous programming has two primary benefits. The first benefit is for end-user GUI programs: asynchronous programming enables responsiveness. The second benefit is for server-side programs: asynchronous programming enables scalability.

Both benefits of asynchronous programming derive from the same underlying aspect: asynchronous programming frees up a thread. For GUI programs, asynchronous programming frees up the UI thread; this permits the GUI application to remain responsive to user input. For server applications, asynchronous programming frees up request threads; this permits the server to use its threads to serve more requests.

> "Asynchronous" means not waiting for something to complete (e.g., sending data over the network to another node), and not making any assumptions about how long it is going to take.<sup>1</sup>

Modern asynchronous .NET applications use two keywords: `async` and `await`. The `async` keyword is added to a method declaration, and performs a double purpose:

1. Enables the `await` keyword within that method.
2. Signals the compiler to generate a state machine for that method, similar to how `yield return` works.

An `async` method may return `Task<TResult>` if it returns a value, `Task` if it doesn't return a value, or any other "task-like" type, such as `ValueTask`. In addition, an `async` method may return `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` if it returns multiple values in an enumeration. The task-like types represent futures; they can notify the calling code when the `async` method completes. Older asynchronous APIs use callbacks or events instead of futures.

## Synchronization context

A **synchronization context** is a mechanism that manages the execution context for asynchronous operations. It helps control how asynchronous callbacks are marshaled between threads. The synchronization context ensures that code scheduled to run on a specific context executes within that context, which is important for scenarios involving user interfaces, where updates to the UI must occur on the UI thread.

The `SynchronizationContext` class is a part of the .NET framework and is designed to provide a way to capture and propagate the execution context. It defines methods like `Post` and `Send` that allow you to send delegates for execution on a specific synchronization context.

For example, in UI applications, like those built using WPF or WinForms, there is typically a synchronization context associated with the UI thread. When asynchronous operations complete, and you need to update the UI based on the results, the synchronization context helps ensure that the UI updates are performed on the UI thread. This is crucial because UI elements are not thread-safe, and updating them from a background thread can lead to unpredictable behavior.

In modern C# asynchronous programming, the `async` and `await` keywords handle synchronization context automatically in many cases. However, it's essential to understand synchronization context when dealing with custom or advanced asynchronous scenarios or when working with older asynchronous patterns that don't inherently support the `async/await` model.

### Example

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

An `async` method begins executing synchronously, just like any other method. Within an `async` method, the `await` keyword performs an _asynchronous wait_ on its argument. First, it checks whether the operation is already complete; if it is, it continues executing (synchronously). Otherwise, it will pause the `async` method and return an incomplete task. When that operation completes some time later, the `async` method will resume executing.

You can think of an `async` method as having several synchronous portions, broken up by `await` statements. The first synchronous portion executes on whatever thread calls the method, but where do the other synchronous portions execute? The answer is a bit complicated.

When you `await` a task (the most common scenario), a _context_ is captured when the `await` decides to pause the method. This is the current `SynchronizationContext` unless it's `null`, in which case the context is the current `TaskScheduler`. The method resumes executing within that captured context. Usually, this context is the UI context (if you're on the UI thread) or the threadpool context (most other situations). If you have an ASP.NET Classic (pre-Core) application, then the context could also be an ASP.NET request context. ASP.NET Core uses the threadpool context rather than a special request context.

So, in the preceding code, all the synchronous portions will attempt to resume on the original context. If you call `DoSomethingAsync` from a UI thread, each of its synchronous portions will run on that UI thread; but if you call it from a threadpool thread, each of its synchronous portions will run on any threadpool thread.

You can avoid this default behavior by awaiting the result of the `ConfigureAwait` extension method and passing `false` for the `continueOnCapturedContext` parameter. The following code will start on the calling thread, and after it is paused by an `await`, it'll resume on a threadpool thread:

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

The `await` keyword is not limited to working with tasks; it can work with any kind of awaitable that follows a certain pattern. As an example, the Base Class Library includes the `ValueTask<T>` type, which reduces memory allocations if the result is commonly synchronous; for example, if the result can be read from an in-memory cache. `ValueTask<T>` is not directly convertible to `Task<T>`, but it does follow the awaitable pattern, so you can directly `await` it. There are other examples, and you can build your own, but most of the time `await` will take a Task or `Task<TResult>`.

There are two basic ways to create a `Task` instance. Some tasks represent actual code that a CPU has to execute; these computational tasks should be created by calling `Task.Run` or `TaskFactory.StartNew` if you need them to run on a particular scheduler). Other tasks represent a notification; these kinds of event-based tasks are created by `TaskCompletionSource<TResult>` or one of its shortcuts. Most I/O tasks use `TaskCompletionSource<TResult>`.

## Deadlock

A **deadlock** is a situation where two or more threads or processes are unable to proceed because each is waiting for the other to release a resource.

There's one other important guideline when it comes to methods: once you start using `async`, it's best to allow it to grow through your code. If you call an `async` method, you should eventually `await` the task it returns. Resist the temptation to call `Task.Wait`, `Task<TResult>.Result`, or `GetAwaiter().GetResult()`; doing so could cause a deadlock. Consider the following method:

```csharp
void Deadlock()
{
    // Start the delay.
    Task task = WaitAsync();
    // Synchronously block, waiting for the async method to complete.
    task.Wait();
}

async Task WaitAsync()
{
    // This await will capture the current context ...
    await Task.Delay(TimeSpan.FromSeconds(1));
    // ... and will attempt to resume the method here in that context.
}
```

The code in this example will deadlock if called from a UI or ASP.NET Classic context because both of those contexts only allow one thread in at a time. `Deadlock` will call `WaitAsync`, which begins the delay. `Deadlock` then synchronously waits for that method to complete, blocking the context thread. When the delay completes, `await` attempts to resume `WaitAsync` within the captured context, but it cannot because there's already a thread blocked in the context, and the context only allows one thread at a time. Deadlock can be prevented two ways: you can use `ConfigureAwait(false)` within `WaitAsync` which causes `await` to ignore its context, or you can `await` the call to `WaitAsync` making `Deadlock` into an `async` method.

## Task.Yield()

You can use `await Task.Yield();` in an asynchronous method to force the method to complete asynchronously. If there is a current synchronization context, this will post the remainder of the method's execution back to that context.

```csharp
Print();
await A();
Print();

static async Task A()
{
    Print();
    await Task.Yield(); // Calling here Task.Delay(1) has almost the same effect.
    Print();
}

static void Print()
{
    Console.WriteLine(Thread.CurrentThread.ManagedThreadId);
}
```

Output:

```console
1
1
7
7
```

## async void

The problem with calling async void is that you don't even get the task back. You have no way of knowing when the function's task has completed. You can't `await` an `async void` function too.

```csharp
try
{
    A();
}
catch (Exception e)
{
    Console.WriteLine("This will never be caught");
}

Thread.Sleep(10);
Console.WriteLine("I may get executed too if Thread.Sleep from above won't take too long");

static async void A()
{
    // await Task.Yield();
    throw new Exception("Ooops");
}
```

```output
Unhandled exception. I may get executed too if Thread.Sleep above won't take too long
System.Exception: Ooops
   at <Program>$.<<<Main>$>g__A|0_0>d.MoveNext() in /Users/aleksei/repositories/examples/ConsoleApp1/Program.cs:line 18
--- End of stack trace from previous location ---
   at System.Threading.Tasks.Task.<>c.<ThrowAsync>b__140_1(Object state)
   at System.Threading.QueueUserWorkItemCallbackDefaultContext.Execute()
   at System.Threading.ThreadPoolWorkQueue.Dispatch()
   at System.Threading._ThreadPoolWaitCallback.PerformWaitCallback()

Process finished with exit code 134.
```

The `async void` function is a "fire and forget": you start the task chain, but you don't care about when it's finished.

When the function returns, all you know is that everything up to the first `await` has executed. Everything after the first `await` will run at some unspecified point in the future that you have no access to.

The bottom line is that an `async void` can crash the system and usually should be used only on the UI side event handlers.

The code below will will work, because raised exception will just:

```csharp
M();

Thread.Sleep(2000);

Console.WriteLine("Still works");

async Task M()
{
    await Task.Delay(1000);
    throw new Exception("Boom!");
}
```

But it you replace `Task` with `void` it won't — the process will crash because of the unhandled exception.

<hr>

<sup>1</sup> Designing Data-Intensive Applications by Martin Kleppmann (2017), p. 553.
