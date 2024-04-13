# Synchronization context, `ConfigureAwait`

A **synchronization context** is a mechanism that manages the execution context for asynchronous operations. It helps control how asynchronous callbacks are marshaled between threads. The synchronization context ensures that code scheduled to run on a specific context executes within that context, which is important for scenarios involving user interfaces, where updates to the UI must occur on the UI thread.

The `SynchronizationContext` class is a part of the .NET framework and is designed to provide a way to capture and propagate the execution context. It defines methods like `Post` and `Send` that allow you to send delegates for execution on a specific synchronization context.

For example, in UI applications, like those built using WPF or WinForms, there is typically a synchronization context associated with the UI thread. When asynchronous operations complete, and you need to update the UI based on the results, the synchronization context helps ensure that the UI updates are performed on the UI thread. This is crucial because UI elements are not thread-safe, and updating them from a background thread can lead to unpredictable behavior.

In modern C# asynchronous programming, the `async` and `await` keywords handle synchronization context automatically in many cases. However, it's essential to understand synchronization context when dealing with custom or advanced asynchronous scenarios or when working with older asynchronous patterns that don't inherently support the `async/await` model.

## Table of contents

- [Synchronization context, `ConfigureAwait`](#synchronization-context-configureawait)
  - [Table of contents](#table-of-contents)
  - [Example](#example)
  - [`ConfigureAwait`](#configureawait)
    - [When should I use `ConfigureAwait(false)`?](#when-should-i-use-configureawaitfalse)
    - [Does `ConfigureAwait(false)` guarantee the callback won't be run in the original context?](#does-configureawaitfalse-guarantee-the-callback-wont-be-run-in-the-original-context)
    - [Is it ok to use `ConfigureAwait(false)` only on the first await in my method and not on the rest?](#is-it-ok-to-use-configureawaitfalse-only-on-the-first-await-in-my-method-and-not-on-the-rest)
    - [Why would I want to use `ConfigureAwait(false)`?](#why-would-i-want-to-use-configureawaitfalse)
    - [Why would I want to use `ConfigureAwait(true)`?](#why-would-i-want-to-use-configureawaittrue)
  - [Links](#links)

## Example



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

## `ConfigureAwait`

### When should I use `ConfigureAwait(false)`?

It depends: are you implementing application-level code or general-purpose library code?

When writing applications, you generally want the default behavior (which is why it is the default behavior). If an app model / environment (e.g. Windows Forms, WPF, ASP.NET Core, etc.) publishes a custom `SynchronizationContext`, there's almost certainly a really good reason it does: it's providing a way for code that cares about synchronization context to interact with the app model / environment appropriately. So if you're writing an event handler in a Windows Forms app, writing a unit test in xunit, writing code in an ASP.NET MVC controller, whether or not the app model did in fact publish a `SynchronizationContext`, you want to use that `SynchronizationContext` if it exists. And that means the default `ConfigureAwait(true)`. You make simple use of `await`, and the right things happen with regards to callbacks/continuations being posted back to the original context if one existed. This leads to the general guidance of: if you're writing app-level code, do not use `ConfigureAwait(false)`. If you think back to the Click event handler code example:

```csharp
private static readonly HttpClient s_httpClient = new HttpClient();

private async void downloadBtn_Click(object sender, RoutedEventArgs e)
{
    string text = await s_httpClient.GetStringAsync("http://example.com/currenttime");
    downloadBtn.Content = text;
}
```

the setting of `downloadBtn.Content = text` needs to be done back in the original context. If the code had violated this guideline and instead used `ConfigureAwait(false)` when it shouldn't have:

```csharp
private static readonly HttpClient s_httpClient = new HttpClient();

private async void downloadBtn_Click(object sender, RoutedEventArgs e)
{
    string text = await s_httpClient.GetStringAsync("http://example.com/currenttime").ConfigureAwait(false); // bug
    downloadBtn.Content = text;
}
```

bad behavior will result. The same would go for code in a classic ASP.NET app reliant on `HttpContext.Current`; using `ConfigureAwait(false)` and then trying to use `HttpContext.Current` is likely going to result in problems.

### Does `ConfigureAwait(false)` guarantee the callback won't be run in the original context?

No. It guarantees it won't be queued back to the original context, but that doesn't mean the code after an `await task.ConfigureAwait(false)` won't still run in the original context. That's because awaits on already-completed awaitables just keep running past the `await` synchronously rather than forcing anything to be queued back. So, if you `await` a task that's already completed by the time it's awaited, regardless of whether you used `ConfigureAwait(false)`, the code immediately after this will continue to execute on the current thread in whatever context is still current.

### Is it ok to use `ConfigureAwait(false)` only on the first await in my method and not on the rest?

In general, no. If the first `await task.ConfigureAwait(false)` involves a task that's already completed by the time it's awaited (which is actually incredibly common), then the `ConfigureAwait(false)` will be meaningless, as the thread continues to execute code in the method after this and still in the same context that was there previously.

One notable exception to this is if you know that the first `await` will always complete asynchronously and the thing being awaited will invoke its callback in an environment free of a custom `SynchronizationContext` or a `TaskScheduler`.

### Why would I want to use `ConfigureAwait(false)`?

`ConfigureAwait(false)` is used to avoid forcing the callback to be invoked on the original context or scheduler. This has a few benefits:

- Performance improvements
- Avoiding potential deadlocks

### Why would I want to use `ConfigureAwait(true)`?

You wouldn't, unless you were using it purely as an indication that you were purposefully not using `ConfigureAwait(false)`, e.g. to silence static analysis warnings or the like. `ConfigureAwait(true)` does nothing meaningful. When comparing `await task` with `await task.ConfigureAwait(true)`, they're functionally identical. If you see `ConfigureAwait(true)` in production code, you can delete it without ill effect.

The `ConfigureAwait` method accepts a Boolean because there are some niche situations in which you want to pass in a variable to control the configuration. But the 99% use case is with a hardcoded `false` argument value, `ConfigureAwait(false)`.

## Links

[↑ ConfigureAwait FAQ](https://devblogs.microsoft.com/dotnet/configureawait-faq/).
