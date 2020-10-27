# Async/await

## Practical advices

- Avoid `.Result()` and `.Wait()`. Use `await` for *asynchronous* code and `.GetAwaiter().GetResult()` for *synchronous* code

- Use `.ConfigureAwait(false)` when the calling thread isn't needed. Most business logic does not need to return to the calling thread

- Avoid `return await`. When `await` is only used in the `return` statement, return the `Task` instead, but don't do this inside of `try/catch` or `using()` blocks.

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

[↑ ConfigureAwait FAQ](https://devblogs.microsoft.com/dotnet/configureawait-faq/)
