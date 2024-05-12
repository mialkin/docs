# Stopwatch

A [↑ `Stopwatch`](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.stopwatch) class provides a set of methods and properties that you can use to accurately measure elapsed time.

In a typical `Stopwatch` scenario, you call the `Start` method, then eventually call the `Stop` method, and then you check elapsed time using the `Elapsed` property:

```csharp
var stopwatch = new Stopwatch();

stopwatch.Start();
Thread.Sleep(1234);
stopwatch.Stop();

var ts = stopwatch.Elapsed;
var elapsedTime = $"{ts.Hours:00}:{ts.Minutes:00}:{ts.Seconds:00}.{ts.Milliseconds / 10:00}";

Console.WriteLine(elapsedTime); // 00:00:01.23
```
