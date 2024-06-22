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
    - [`ParallelQuery<TSource>`](#parallelquerytsource)
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

PLINQ automates all the steps of parallelization — including partitioning the work into tasks, executing those tasks on threads, and collating the results into a single output sequence. It’s called _declarative_ — because you simply declare that you want to parallelize your work, which you structure as a LINQ query, and let the Framework take care of the implementation details. In contrast, the other approaches are _imperative_, in that you need to explicitly write code to partition or collate. In the case of the `Parallel` class, you must collate results yourself; with the task parallelism constructs, you must partition the work yourself, too:

| API              | Partitions work | Collates results |
| ---------------- | --------------- | ---------------- |
| PLINQ            | Yes             | Yes              |
| `Parallel` class | Yes             | No               |
| Task parallelism | No              | No               |

### `ParallelQuery<TSource>`

The [↑ `ParallelQuery<TSource>`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelquery-1) class represents a parallel sequence.

### `ParallelEnumerable`

The [↑ `ParallelEnumerable`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable) class provides a set of methods for querying objects that implement [`ParallelQuery<TSource>`](#parallelquerytsource). This is the parallel equivalent of [↑ `Enumerable`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.enumerable).

#### `AsParallel`

To use PLINQ, simply call `AsParallel()` on the input sequence and then continue the LINQ query as usual.

```csharp
var letters = new[] { "a", "b", "c", "d", "e", "f" };

Console.WriteLine(string.Join(", ", letters.AsParallel().Select(x => x.ToUpper())));
// Output:
// C, D, E, A, B, F
```

`AsParallel()` wraps the input in a sequence based on [`ParallelQuery<TSource>`](#parallelquerytsource), which causes the LINQ query operators that you subsequently call to bind to an alternate set of extension methods defined in [`ParallelEnumerable`](#parallelenumerable). These provide parallel implementations of each of the standard query operators. Essentially, they work by partitioning the input sequence into chunks that execute on different threads, collating the results back into a single output sequence for consumption.

## `Parallel`

The [↑ `Parallel`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.parallel) class provides support for parallel loops and regions.

## `AggregateException`

The [↑ `AggregateException`](https://learn.microsoft.com/en-us/dotnet/api/system.aggregateexception) class represents one or more errors that occur during application execution.
