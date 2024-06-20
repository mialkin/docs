# `Stopwatch`, `IProgress<T>`

## Table of contents

- [`Stopwatch`, `IProgress<T>`](#stopwatch-iprogresst)
  - [Table of contents](#table-of-contents)
  - [`Stopwatch`](#stopwatch)
  - [`PeriodicTimer`](#periodictimer)
  - [`IProgress<T>`](#iprogresst)

## `Stopwatch`

The [↑ `Stopwatch`](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.stopwatch) class provides a set of methods and properties that you can use to accurately measure elapsed time.

In a typical `Stopwatch` scenario, you call the `Start` method, then eventually call the `Stop` method, and then you check elapsed time using the `Elapsed` property:

```csharp
var stopwatch = new Stopwatch();

stopwatch.Start();
Thread.Sleep(1234);
stopwatch.Stop();

var timeSpan = stopwatch.Elapsed;
var elapsedTime = $"{timeSpan.Hours:00}:{timeSpan.Minutes:00}:{timeSpan.Seconds:00}.{timeSpan.Milliseconds / 10:00}";

Console.WriteLine(elapsedTime); // 00:00:01.23
```

## `PeriodicTimer`

The `PeriodicTimer` class provides a periodic timer that enables waiting asynchronously for timer ticks.

[↑ Scheduling repeating tasks with .NET 6's new timer](https://www.youtube.com/watch?v=J4JL4zR_l-0).

## `IProgress<T>`

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

[↑ Have You Ever Signaled async/await Progress in These Three Ways?](https://www.youtube.com/watch?v=dhleFJPOQOs).
