# `dotnet-dump`, `dotnet-counters`, `dotnet-trace`

## `dotnet-dump`

The [↑ `dotnet-dump`](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/dotnet-dump) is a dump collection and analysis utility.

[↑ Finding memory leaks in C# .NET Applications](https://www.youtube.com/watch?v=9QPgfJPaGvY).

## `dotnet-counters`

The `dotnet-counters` is a performance monitoring tool for ad-hoc health monitoring and first-level performance investigation.

The tool can observe performance counter values that are published via the [↑ EventCounter](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.tracing.eventcounter) API or the [↑ Meter API](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.metrics.meter). For example, you can quickly monitor things like the CPU usage or the rate of exceptions being thrown in your .NET Core application to see if there's anything suspicious before diving into more serious performance investigation using `dotnet-trace` or [↑ PerfView](https://habr.com/ru/companies/skbkontur/articles/723010/).

- [↑ ASP.NET Core Series: Performance Testing Techniques](https://www.youtube.com/watch?v=jn54CjePzs0)

## `dotnet-trace`

The [↑ `dotnet-trace`](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/dotnet-trace) is tool that enables the collection of .NET Core traces of a running process without a native profiler.

The tool is built on [↑ EventPipe](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/eventpipe) of the .NET Core runtime.
