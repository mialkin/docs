# Reporting progress

If you need to respond to progress while an operation is executing use the provided [↑ `IProgress<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.iprogress-1) and [↑ `Progress<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.progress-1) types.

Your `async` method should take an `IProgress<T>` argument; the `T` is whatever type of progress you need to report:

```csharp
var workProgress = new Progress<double>();
workProgress.ProgressChanged += (sender, value) => Console.WriteLine($"Progress: {Math.Round(value, 2)}%");

await DoWorkAsync(workProgress);

async Task DoWorkAsync(IProgress<double>? progress = null)
{
    for (int i = 0; i < 7; i++)
    {
        await Task.Delay(500);

        var completionPercentage = (double)(i + 1) / 7 * 100;

        progress?.Report(completionPercentage);
    }
}
```

By convention, the `IProgress<T>` parameter may be `null` if the caller doesn't need progress reports, so be sure to check for this in your `async` method.

`IProgress<T>` is not exclusively for asynchronous code; both progress and cancellation can (and should) be used in long-running synchronous code as well.
