# Signaling, event wait handle, `AutoResetEvent`, `ManualResetEvent`, `ManualResetEventSlim`

A **signaling** is a mechanism that makes a thread to wait until it receives notification from another thread.

An **event wait handle** is a construct used for signaling.

Event wait handles are the simplest of the signaling constructs, and they are unrelated to C# [events](/csharp/keywords/event.md). They come in three flavors: [`AutoResetEvent`](#autoresetevent), [`ManualResetEvent`](#manualresetevent), and [`CountdownEvent`](#countdownevent). The former two are based on the common [↑ `EventWaitHandle`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.eventwaithandle) class, where they derive all their functionality.

## Table of contents

- [Signaling, event wait handle, `AutoResetEvent`, `ManualResetEvent`, `ManualResetEventSlim`](#signaling-event-wait-handle-autoresetevent-manualresetevent-manualreseteventslim)
  - [Table of contents](#table-of-contents)
  - [`AutoResetEvent`](#autoresetevent)
  - [`ManualResetEvent`](#manualresetevent)
  - [`ManualResetEventSlim`](#manualreseteventslim)
  - [`CountdownEvent`](#countdownevent)

## `AutoResetEvent`

The [↑ `AutoResetEvent`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.autoresetevent) class is a thread synchronization event that, when signaled, releases one single waiting thread and then resets automatically.

## `ManualResetEvent`

The [↑ `ManualResetEvent`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.manualresetevent) class is a thread synchronization event that, when signaled, must be reset manually.

## `ManualResetEventSlim`

The [↑ `ManualResetEventSlim`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.manualreseteventslim) class is a thread synchronization event that, when signaled, must be reset manually. This class is a lightweight alternative to [`ManualResetEvent`](#manualresetevent).

## `CountdownEvent`

The [↑ `CountdownEvent`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.countdownevent) class is a synchronization primitive that is signaled when its count reaches zero.
