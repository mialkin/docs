# `dotnet-counters`, `dotnet-dump`, `dotnet-trace`

## Table of contents

- [`dotnet-counters`, `dotnet-dump`, `dotnet-trace`](#dotnet-counters-dotnet-dump-dotnet-trace)
  - [Table of contents](#table-of-contents)
  - [`dotnet-counters`](#dotnet-counters)
    - [`dotnet-counters monitor`](#dotnet-counters-monitor)
  - [`dotnet-dump`](#dotnet-dump)
    - [Analyze SOS commands](#analyze-sos-commands)
  - [`dotnet-trace`](#dotnet-trace)

## `dotnet-counters`

[↑ `dotnet-counters`](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/dotnet-counters) is a performance monitoring tool for ad-hoc health monitoring and first-level performance investigation.

```bash
dotnet tool install --global dotnet-counters
```

The tool can observe performance counter values that are published via the [↑ EventCounter](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.tracing.eventcounter) API or the [↑ Meter API](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.metrics.meter). For example, you can quickly monitor things like the CPU usage or the rate of exceptions being thrown in your .NET Core application to see if there's anything suspicious before diving into more serious performance investigation using [`dotnet-trace`](#dotnet-trace) or [↑ PerfView](https://habr.com/ru/companies/skbkontur/articles/723010/).

[↑ `dotnet-counters ps`](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/dotnet-counters#dotnet-counters-ps) — lists the dotnet processes that can be monitored by `dotnet-counters`, also display the command-line arguments that each process was started with, if available:

```bash
dotnet-counters ps
```

### `dotnet-counters monitor`

[↑ `dotnet-counters monitor`](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/dotnet-counters#dotnet-counters-monitor) — displays periodically refreshing values of selected counters.

```bash
dotnet-counters monitor --name MyApp.Api
```

or:

```bash
dotnet-counters monitor --process-id 1234
```

## `dotnet-dump`

[↑ `dotnet-dump`](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/dotnet-dump) is a dump collection and analysis utility.

```bash
dotnet tool install --global dotnet-dump
```

[↑ `dotnet-dump ps`](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/dotnet-dump#dotnet-dump-ps) — lists the dotnet processes that dumps can be collected from, also displays the command-line arguments that each process was started with, if available.

```bash
dotnet-dump ps
```

[↑ `dotnet-dump collect`](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/dotnet-dump#dotnet-dump-collect) command captures a dump from a process.

```bash
dotnet-dump analyze /path/to/dump_file
```

### Analyze SOS commands

`dumpheap` — displays info about the garbage-collected heap and collection statistics about objects.

```bash
dumpheap
```

`dumpheap -mt MEMORY_VIRTUAL_ADDRESS` — zoom into object at virtual address.

```bash
dumpheap -mt 00010d6dd7a8
```

`dumpheap -type TYPE_NAME` — filter by type name.

```bash
dumpheap -type Memory.Api.Endpoints.Lohs.GenerateLargeStrings.GenerateLargeStringsEndpoint  
```

`gcroot MEMORY_VIRTUAL_ADDRESS` — displays info about references (or roots) to the object at the specified address.

```bash
gcroot 0001978b9470 
```

## `dotnet-trace`

[↑ `dotnet-trace`](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/dotnet-trace) is tool that enables the collection of .NET Core traces of a running process without a native profiler.

The tool is built on [↑ EventPipe](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/eventpipe) of the .NET Core runtime.
