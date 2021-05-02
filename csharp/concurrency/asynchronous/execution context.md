# Execution context

## Content

In the synchronous world, each thread keeps ambient information in a *thread-local storage*<sup>1</sup>. It can be security-related information, culture-specific data, or something else. When 3 methods are called sequentially in one thread this information flows naturally between all of them. But this is no longer true for asynchronous methods. Each "section" of an asynchronous method can be executed in different threads that makes thread-local information unusable.

Execution context keeps the information for one logical *flow of control*<sup>3</sup> even when it spans multiple threads.

Methods like `Task.Run` or `ThreadPool.QueueUserWorkItem` do this automatically. `Task.Run` method captures `ExecutionContext` from the invoking thread and stores it with the `Task` instance. When the `TaskScheduler` associated with the task runs a given delegate, it runs it via `ExecutionContext.Run` using the stored context.

We can use [↑ AsyncLocal<T>](https://docs.microsoft.com/en-us/dotnet/api/system.threading.asynclocal-1) to demonstrate this concept in action:

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

class Example
{
    static async Task Main()
    {
        await ExecutionContextInAction();
    }

    static Task ExecutionContextInAction()
    {
        var li = new AsyncLocal<int> { Value = 42 };

        return Task.Run(() =>
        {
            // Task.Run restores the execution context
            Console.WriteLine("In Task.Run: " + li.Value);
        }).ContinueWith(_ =>
        {
            // The continuation restores the execution context as well
            Console.WriteLine("In Task.ContinueWith: " + li.Value);
        });
    }
}
```

In these cases, the execution context flows through `Task.Run` and then to `Task.ContinueWith` method. So if you run this method you’ll see:

```output
In Task.Run: 42
In Task.ContinueWith: 42
```

But not all methods in the BCL will automatically capture and restore the execution context. Two exceptions are `TaskAwaiter<T>.UnsafeOnComplete` and `AsyncMethodBuilder<T>.AwaitUnsafeOnComplete`. It looks weird that the language authors decided to add "unsafe" methods to flow the execution context manually using `AsyncMethodBuilder<T>` and `MoveNextRunner` instead of relying on a built-in facilities like `AwaitTaskContinuation`. I suspect there were some performance reasons or another restrictions on the existing implementation.

Here is an example that demonstrates the difference:

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

class Example
{
    static async Task Main()
    {
        await ExecutionContextInAsyncMethod();
    }

    static async Task ExecutionContextInAsyncMethod()
    {
        var li = new AsyncLocal<int> { Value = 42 };
        await Task.Delay(42);

        // The context is implicitly captured. li.Value is 42
        Console.WriteLine("After first await: " + li.Value);

        var tsk2 = Task.Yield();
        tsk2.GetAwaiter().UnsafeOnCompleted(() =>
        {
            // The context is not captured: li.Value is 0
            Console.WriteLine("Inside UnsafeOnCompleted: " + li.Value);
        });

        await tsk2;

        // The context is captured: li.Value is 42
        Console.WriteLine("After second await: " + li.Value);
    }
}
```

The output is:

```output
After first await: 42
Inside UnsafeOnCompleted: 0
After second await: 42
```

## Footnotes

1. The **thread-local storage** is a mechanism by which each thread in a given multithreaded process allocates storage for thread-specific data.

2. The **flow of control** is the order in which individual statements, instructions or function calls are executed or evaluated.

## Links

* [↑ Dissecting the async methods in C#](https://devblogs.microsoft.com/premier-developer/dissecting-the-async-methods-in-c/)
* [↑ ExecutionContext Class](https://docs.microsoft.com/en-us/dotnet/api/system.threading.executioncontext)
