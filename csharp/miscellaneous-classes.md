# `Stopwatch`, `PeriodicTimer`, `Progress<T>`, `StreamWriter`

## Table of contents

- [`Stopwatch`, `PeriodicTimer`, `Progress<T>`, `StreamWriter`](#stopwatch-periodictimer-progresst-streamwriter)
  - [Table of contents](#table-of-contents)
  - [`Stopwatch`](#stopwatch)
  - [`PeriodicTimer`](#periodictimer)
  - [`Progress<T>`](#progresst)
  - [`StreamWriter`](#streamwriter)

## `Stopwatch`

The [↑ `Stopwatch`](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.stopwatch) class provides a set of methods and properties that you can use to accurately measure elapsed time.

In a typical `Stopwatch` scenario, you call the `Start` method, then eventually call the `Stop` method, and then you check elapsed time using the `Elapsed` property:

```csharp
var startTime = Stopwatch.GetTimestamp();
Thread.Sleep(1234);
var timeSpan = Stopwatch.GetElapsedTime(startTime);

var elapsedTime = $"{timeSpan.Hours:00}:{timeSpan.Minutes:00}:{timeSpan.Seconds:00}.{timeSpan.Milliseconds / 10:00}";
Console.WriteLine(elapsedTime); // 00:00:01.23
```

## `PeriodicTimer`

The [↑ `PeriodicTimer`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.periodictimer) class provides a periodic timer that enables waiting asynchronously for timer ticks.

[↑ Scheduling repeating tasks with .NET 6's new timer](https://www.youtube.com/watch?v=J4JL4zR_l-0).

```csharp
public class SampleBackgroundService(ILogger<SampleBackgroundService> logger)
    : Microsoft.Extensions.Hosting.BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        using PeriodicTimer timer = new(TimeSpan.FromMinutes(1));

        while (!stoppingToken.IsCancellationRequested)
        {
            try
            {
                var duration = TimeSpan.FromSeconds(Random.Shared.Next(10, 50));
                logger.LogInformation("Doing some business logic during {Duration}", duration);
                await Task.Delay(duration, stoppingToken);
                logger.LogInformation("Finished doing some business logic during {Duration}", duration);
            }
            catch (OperationCanceledException)
            {
                logger.LogInformation("Timed hosted service is stopping");
            }
            catch (Exception exception)
            {
                logger.LogError(exception, "Error occurred");
            }

            await timer.WaitForNextTickAsync(stoppingToken);
        }
    }
}
```

## `Progress<T>`

The [↑ `Progress<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.progress-1) class provides an [↑ `IProgress<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.iprogress-1) that invokes callbacks for each reported progress value.

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

`IProgress<T>` is not exclusively for asynchronous code; both progress and cancellation should be used in long-running synchronous code as well.

[↑ Have You Ever Signaled async/await Progress in These Three Ways?](https://www.youtube.com/watch?v=dhleFJPOQOs).

## `StreamWriter`

The [↑ `StreamWriter`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter) class implements a [↑ `TextWriter`](https://learn.microsoft.com/en-us/dotnet/api/system.io.textwriter) for writing characters to a stream in a particular encoding.

Counting number of times the file was opened and written to in one second:

```csharp
var stopwatch = new Stopwatch();
stopwatch.Start();
var counter = 0;

while (stopwatch.ElapsedMilliseconds < 1000)
{
    using (var writer = new StreamWriter(path: "file.txt", append: true))
    {
        writer.WriteLine("test");
    }

    counter++;
}

stopwatch.Stop();
Console.WriteLine(counter);

// Output:
// 14780
```
