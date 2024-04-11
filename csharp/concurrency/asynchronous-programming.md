# Asynchronous programming

- [Asynchronous programming](#asynchronous-programming)
    - [Example](#example)


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
