# `CountdownEvent`

`CountdownEvent` is a synchronization primitive that unblocks its waiting threads after it has been signaled a certain number of times. `CountdownEvent` is designed for scenarios in which you would otherwise have to use a `ManualResetEvent` or `ManualResetEventSlim` and manually decrement a variable before signaling the event. For example, in a fork/join scenario, you can just create a `CountdownEvent` that has a signal count of 5, and then start five work items on the thread pool and have each work item call `Signal` when it completes. Each call to `Signal` decrements the signal count by 1. On the main thread, the call to `Wait` will block until the signal count is zero.

> For code that does not have to interact with legacy .NET Framework synchronization APIs, consider using `Task` objects or the `Invoke` method for an even easier approach to expressing fork-join parallelism.

```csharp
CountdownEvent cde = new CountdownEvent(2);

Task.Run(() => Signal(10));
Task.Run(() => Signal(2));

Console.WriteLine("Started waiting");   // Prints out immediately
cde.Wait();

// It's good to release a CountdownEvent when you're done with it.
cde.Dispose();

Console.WriteLine("Finished waiting");  // Prints out in 10 seconds

async Task Signal(int delay)
{
    await Task.Delay(TimeSpan.FromSeconds(delay));
    cde.Signal();
}
```

There are `CurrentCount`, `InitialCount` properties and `AddCount` method:

```csharp
CountdownEvent cde = new CountdownEvent(2);

Console.WriteLine(cde.CurrentCount); // 2
Console.WriteLine(cde.InitialCount); // 2

cde.AddCount(4);

Console.WriteLine(cde.CurrentCount); // 6
Console.WriteLine(cde.InitialCount); // 2

cde.Signal();

Console.WriteLine(cde.CurrentCount); // 5
Console.WriteLine(cde.InitialCount); // 2
```

All public and protected members of `CountdownEvent` are thread-safe and may be used concurrently from multiple threads, with the exception of `Dispose()`, which must only be used when all other operations on the `CountdownEvent` have completed, and `Reset()`, which should only be used when no other threads are accessing the event.

## Links

[↑ CountdownEvent](https://docs.microsoft.com/en-us/dotnet/standard/threading/countdownevent)
