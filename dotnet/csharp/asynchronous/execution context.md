# Execution context

One may wonder: what is the execution context and why we need all that complexity?

In the synchronous world, each thread keeps ambient information in a thread-local storage. It can be security-related information, culture-specific data, or something else. When 3 methods are called sequentially in one thread this information flows naturally between all of them. But this is no longer true for asynchronous methods. Each “section” of an asynchronous method can be executed in different threads that makes thread-local information unusable.

Execution context keeps the information for one logical flow of control even when it spans multiple threads.

Methods like `Task.Run` or `ThreadPool.QueueUserWorkItem` do this automatically. `Task.Run` method captures `ExecutionContext` from the invoking thread and stores it with the `Task` instance. When the TaskScheduler associated with the task runs a given delegate, it runs it via ExecutionContext.Run using the stored context.

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

Output:

```output
In Task.Run: 42
In Task.ContinueWith: 42
```

## Links

[↑ ExecutionContext Class](https://docs.microsoft.com/en-us/dotnet/api/system.threading.executioncontext)
