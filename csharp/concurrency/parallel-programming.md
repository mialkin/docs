# Parallel programming, PFX, TPL, PLINQ, `Parallel`, `AggregateException`

A **parallel programming** is a programming that leverages multiple cores or multiple processors. This is a subset of the broader concept of _multithreading_.

There are two strategies for partitioning work among threads: _data parallelism_ and _task parallelism_.

A **data parallelism** is a partitioning the data between threads.

A **task parallelism** is a partitioning the tasks between threads, i.e. each thread performs a different task.

## Table of contents

- [Parallel programming, PFX, TPL, PLINQ, `Parallel`, `AggregateException`](#parallel-programming-pfx-tpl-plinq-parallel-aggregateexception)
  - [Table of contents](#table-of-contents)
  - [PFX](#pfx)
  - [TPL](#tpl)
  - [PLINQ](#plinq)
    - [`ParallelEnumerable`](#parallelenumerable)
      - [`AsParallel`](#asparallel)
  - [`Parallel`](#parallel)
  - [`AggregateException`](#aggregateexception)

## PFX

**Parallel Framework** or **PFX** is a set of APIs:

- [PLINQ](#plinq)
- [`Parallel`](#parallel) class
- The task parallelism constructs
- The [concurrent collections](collections.md#concurrent-collections)
- [`SpinLock`](synchronization/locking.md#spinlock) and [`SpinWait`](synchronization/locking.md#spinwait)

## TPL

Task Parallel Library or TPL is is a subset of PFX:

- [`Parallel`](#parallel) class
- The task parallelism constructs

## PLINQ

The [↑ Parallel LINQ](https://learn.microsoft.com/en-us/dotnet/standard/parallel-programming/introduction-to-plinq) or **PLINQ**, is a parallel execution engine for [LINQ](/csharp/linq.md) expressions.

PLINQ automatically parallelizes local LINQ queries.

### `ParallelEnumerable`

The [↑ `ParallelEnumerable`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable) class provides a set of methods for querying objects that implement [↑ `ParallelQuery<TSource>`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelquery-1). This is the parallel equivalent of [↑ `Enumerable`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.enumerable).

#### `AsParallel`

To use PLINQ, simply call `AsParallel()` on the input sequence and then continue the LINQ query as usual.

## `Parallel`

The [↑ `Parallel`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.parallel) class provides support for parallel loops and regions.

## `AggregateException`

The [↑ `AggregateException`](https://learn.microsoft.com/en-us/dotnet/api/system.aggregateexception) class represents one or more errors that occur during application execution.
