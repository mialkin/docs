# Async/await best practices

## Practical advices

* Avoid `.Result()` and `.Wait()`. Use `await` for *asynchronous* code and `.GetAwaiter().GetResult()` for *synchronous* code. `.GetAwaiter().GetResult()` is effectively the same as `.Wait()`, it's going to block, but it will unwrap the exception, if the exception is thrown inside running method.

* Use `.ConfigureAwait(false)` when the calling thread isn't needed. Most business logic does not need to return to the calling thread.

* Avoid `return await`. When `await` is only used in the `return` statement, return the `Task` instead, but don't do this inside of `try/catch` or `using()` blocks.

## When should I use ConfigureAwait(false)?

It depends: are you implementing application-level code or general-purpose library code?

When writing applications, you generally want the default behavior (which is why it is the default behavior). If an app model / environment (e.g. Windows Forms, WPF, ASP.NET Core, etc.) publishes a custom `SynchronizationContext`, there’s almost certainly a really good reason it does: it’s providing a way for code that cares about synchronization context to interact with the app model / environment appropriately. So if you’re writing an event handler in a Windows Forms app, writing a unit test in xunit, writing code in an ASP.NET MVC controller, whether or not the app model did in fact publish a `SynchronizationContext`, you want to use that `SynchronizationContext` if it exists. And that means the default `ConfigureAwait(true)`. You make simple use of `await`, and the right things happen with regards to callbacks/continuations being posted back to the original context if one existed. This leads to the general guidance of: if you’re writing app-level code, do not use `ConfigureAwait(false)`. If you think back to the Click event handler code example:

```csharp
private static readonly HttpClient s_httpClient = new HttpClient();

private async void downloadBtn_Click(object sender, RoutedEventArgs e)
{
    string text = await s_httpClient.GetStringAsync("http://example.com/currenttime");
    downloadBtn.Content = text;
}
```

the setting of `downloadBtn.Content = text` needs to be done back in the original context. If the code had violated this guideline and instead used `ConfigureAwait(false)` when it shouldn’t have:

```csharp
private static readonly HttpClient s_httpClient = new HttpClient();

private async void downloadBtn_Click(object sender, RoutedEventArgs e)
{
    string text = await s_httpClient.GetStringAsync("http://example.com/currenttime").ConfigureAwait(false); // bug
    downloadBtn.Content = text;
}
```

bad behavior will result. The same would go for code in a classic ASP.NET app reliant on `HttpContext.Current`; using ConfigureAwait(false) and then trying to use `HttpContext.Current` is likely going to result in problems.

## Does ConfigureAwait(false) guarantee the callback won’t be run in the original context?

No. It guarantees it won’t be queued back to the original context, but that doesn’t mean the code after an `await task.ConfigureAwait(false)` won’t still run in the original context. That’s because awaits on already-completed awaitables just keep running past the `await` synchronously rather than forcing anything to be queued back. So, if you `await` a task that’s already completed by the time it’s awaited, regardless of whether you used `ConfigureAwait(false)`, the code immediately after this will continue to execute on the current thread in whatever context is still current.

## Is it ok to use ConfigureAwait(false) only on the first await in my method and not on the rest?

In general, no. If the `await task.ConfigureAwait(false)` involves a task that’s already completed by the time it’s awaited (which is actually incredibly common), then the `ConfigureAwait(false)` will be meaningless, as the thread continues to execute code in the method after this and still in the same context that was there previously.

One notable exception to this is if you know that the first await will always complete asynchronously and the thing being awaited will invoke its callback in an environment free of a custom `SynchronizationContext` or a `TaskScheduler`.

## Links

* [↑ ConfigureAwait FAQ](https://devblogs.microsoft.com/dotnet/configureawait-faq/)
* [↑ Long Story Short: Async/Await Best Practices in .NET](https://medium.com/@deep_blue_day/long-story-short-async-await-best-practices-in-net-1f39d7d84050)
* [↑ Дмитрий Иванов — Async programming in .NET: Best practices](https://www.youtube.com/watch?v=wM-h6P1BJRk)
