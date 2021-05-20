# Stopwatch

A `Stopwatch` instance can measure elapsed time for one interval, or the total of elapsed time across multiple intervals. In a typical `Stopwatch` scenario, you call the `Start` method, then eventually call the `Stop` method, and then you check elapsed time using the `Elapsed` property.

```csharp
using System;
using System.Diagnostics;
using System.Threading;

Stopwatch stopWatch = new();
stopWatch.Start();

Thread.Sleep(1234);

stopWatch.Stop();
TimeSpan ts = stopWatch.Elapsed;
string elapsedTime = $"{ts.Hours:00}:{ts.Minutes:00}:{ts.Seconds:00}.{ts.Milliseconds / 10:00}";

Console.WriteLine("RunTime " + elapsedTime);
```

Output:

```terminal
RunTime 00:00:01.23
```
