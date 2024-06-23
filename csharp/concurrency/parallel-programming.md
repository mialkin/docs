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
      - [`AsSequential`](#assequential)
      - [`AsOrdered`](#asordered)
      - [`AsUnordered`](#asunordered)
      - [`WithExecutionMode`](#withexecutionmode)
    - [Example](#example)
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

PLINQ automates all the steps of parallelization — including partitioning the work into tasks, executing those tasks on threads, and collating the results into a single output sequence. It's called _declarative_ — because you simply declare that you want to parallelize your work, which you structure as a LINQ query, and let the Framework take care of the implementation details. In contrast, the other approaches are _imperative_, in that you need to explicitly write code to partition or collate. In the case of the `Parallel` class, you must collate results yourself; with the task parallelism constructs, you must partition the work yourself, too:

| API              | Partitions work | Collates results |
| ---------------- | --------------- | ---------------- |
| PLINQ            | Yes             | Yes              |
| `Parallel` class | Yes             | No               |
| Task parallelism | No              | No               |

If a PLINQ query throws an exception, it's rethrown as an [`AggregateException`](#aggregateexception) whose `InnerExceptions` property contains the real exception(-s).

Like ordinary LINQ queries, PLINQ queries are lazily evaluated. This means that execution is triggered only when you begin consuming the results — typically via a `foreach` loop, although it may also be via a conversion operator such as `ToArray` or an operator that returns a single element or value.

A parallel query ordinarily uses independent threads to fetch elements from the input sequence slightly _ahead_ of when they're needed by the consumer, rather like a teleprompter for newsreaders, or an [↑ antiskip buffer](https://en.wikipedia.org/wiki/Electronic_skip_protection) in CD players. It then processes the elements in parallel through the query chain, holding the results in a small buffer so that they're ready for the consumer on demand. If the consumer pauses or breaks out of the enumeration early, the query processor also pauses or stops so as not to waste CPU time or memory.

You can tweak PLINQ's buffering behavior by calling [↑ `WithMergeOptions`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.withmergeoptions) after `AsParallel`. The default value of `AutoBuffered` generally gives the best overall results. `NotBuffered` disables the buffer and is useful if you want to see results as soon as possible; `FullyBuffered` caches the entire result set before presenting it to the consumer, the `OrderBy` and `Reverse` operators naturally work this way, as do the element, aggregation, and conversion operators.

### `ParallelQuery<TSource>`

The [↑ `ParallelQuery<TSource>`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelquery-1) class represents a parallel sequence.

### `ParallelEnumerable`

The [↑ `ParallelEnumerable`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable) class provides a set of methods for querying objects that implement [`ParallelQuery<TSource>`](#parallelquerytsource). This is the parallel equivalent of [↑ `Enumerable`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.enumerable).

#### `AsParallel`

The [↑ `AsParallel`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.asparallel) method enables parallelization of a query.

To use PLINQ, simply call `AsParallel()` on the input sequence and then continue the LINQ query as usual.

```csharp
var letters = new[] { "a", "b", "c", "d", "e", "f" };
Console.WriteLine(string.Join(", ", letters.AsParallel().Select(x => x.ToUpper())));
// Output:
// C, D, E, A, B, F
```

`AsParallel()` wraps the input in a sequence based on [`ParallelQuery<TSource>`](#parallelquerytsource), which causes the LINQ query operators that you subsequently call to bind to an alternate set of extension methods defined in [`ParallelEnumerable`](#parallelenumerable). These provide parallel implementations of each of the standard query operators. Essentially, they work by partitioning the input sequence into chunks that execute on different threads, collating the results back into a single output sequence for consumption.

#### `AsSequential`

The [↑ `AsSequential`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.assequential) method converts a [`ParallelQuery<TSource>`](#parallelquerytsource) into an `IEnumerable<T>` to force sequential evaluation of the query.

```csharp
var letters = new[] { "a", "b", "c", "d", "e", "f" };
Console.WriteLine(string.Join(", ", letters.AsParallel().AsSequential().Select(x => x.ToUpper())));
// Output:
// A, B, C, D, E, F
```

Calling `AsSequential()` unwraps a `ParallelQuery` sequence so that subsequent query operators bind to the standard query operators and execute sequentially. This is necessary before calling methods that have side effects or are not thread-safe.

#### `AsOrdered`

The [↑ `AsOrdered`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.asordered) method enables treatment of a data source as if it were ordered, overriding the default of unordered.

A side effect of parallelizing the query operators is that when the results are collated, it's not necessarily in the same order that they were submitted, as illustrated in the previous diagram. In other words, LINQ's normal order-preservation guarantee for sequences no longer holds.

```csharp
var letters = new[] { "a", "b", "c", "d", "e", "f" };
Console.WriteLine(string.Join(", ", letters.AsParallel().AsOrdered().Select(x => x.ToUpper())));
// Output:
// A, B, C, D, E, F
```

Calling `AsOrdered` incurs a performance hit with large numbers of elements because PLINQ must keep track of each element's original position.

`AsOrdered` may only be invoked on non-generic sequences returned by [`AsParallel`](#asparallel), `ParallelEnumerable.Range`, and `ParallelEnumerable.Repeat`.

#### `AsUnordered`

The [↑ `AsUnordered`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.asunordered) method allows an intermediate query to be treated as if no ordering is implied among the elements.

You can negate the effect of `AsOrdered` later in a query by calling `AsUnordered`: this introduces a "random shuffle point" which allows the query to execute more efficiently from that point on.

```csharp
inputSequence.AsParallel().AsOrdered()
    .QueryOperator1()
    .QueryOperator2()
    .AsUnordered() // From here on, ordering doesn't matter
    .QueryOperator3()
```

#### `WithExecutionMode`

The [↑ `WithExecutionMode<TSource>`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.withexecutionmode) sets the execution mode of the query.

PLINQ may run your query sequentially if it suspects that the overhead of parallelization will slow down that particular query. You can override this behavior and force parallelism by calling `.WithExecutionMode(ParallelExecutionMode.ForceParallelism` after `AsParallel()`:

### Example

```csharp
[ThreadingDiagnoser]
[MemoryDiagnoser]
[Orderer(SummaryOrderPolicy.FastestToSlowest)]
public class Benchmark
{
    private string[] _randomText;
    private HashSet<string> _hashSet;

    [GlobalSetup]
    public async Task Setup()
    {
        const string fileName = "WordLookup.txt";

        // Download a dictionary of English words into a HashSet for efficient lookup.
        if (!File.Exists(fileName))
        {
            using var client = new HttpClient();
            var result = await client.GetStringAsync("https://www.albahari.com/ispell/allwords.txt");
            await File.WriteAllTextAsync(fileName, result);
        }

        _hashSet = new HashSet<string>(
            collection: await File.ReadAllLinesAsync(fileName),
            comparer: StringComparer.InvariantCultureIgnoreCase);

        // Create a test "document" comprising an array of a million random words.
        // After building the array, introduce a couple of spelling mistakes.
        var wordList = _hashSet.ToArray();
        _randomText = Enumerable.Range(0, 1_000_000)
            .Select(_ => wordList[Random.Shared.Next(0, wordList.Length)]).ToArray();

        _randomText[12345] = "appple"; // Introduce a couple
        _randomText[23456] = "vatermelon"; // of spelling mistakes.
    }

    [Benchmark]
    public void Parallel()
    {
        var wordsWithMistakes = _randomText
            .AsParallel()
            .Select((x, index) => new IndexedWord { Word = x, Index = index })
            .Where(x => !_hashSet.Contains(x.Word))
            .OrderBy(x => x.Index);

        var count = wordsWithMistakes.Count(); // Force enumeration
    }

    [Benchmark]
    public void Sequential()
    {
        var wordsWithMistakes = _randomText
            .Select((x, index) => new IndexedWord { Word = x, Index = index })
            .Where(x => !_hashSet.Contains(x.Word))
            .OrderBy(x => x.Index);

        var count = wordsWithMistakes.Count(); // Force enumeration
    }

    [Benchmark]
    public void ParallelClass()
    {
        var wordsWithMistakes = _randomText
            .AsParallel()
            .Select((x, index) => new IndexedWordClass { Word = x, Index = index })
            .Where(x => !_hashSet.Contains(x.Word))
            .OrderBy(x => x.Index);

        var count = wordsWithMistakes.Count(); // Force enumeration
    }

    [Benchmark]
    public void SequentialClass()
    {
        var wordsWithMistakes = _randomText
            .Select((x, index) => new IndexedWordClass { Word = x, Index = index })
            .Where(x => !_hashSet.Contains(x.Word))
            .OrderBy(x => x.Index);

        var count = wordsWithMistakes.Count(); // Force enumeration
    }
}
```

```csharp
struct IndexedWord
{
    public string Word { get; set; }
    public int Index { get; set; }
}

class IndexedWordClass
{
    public string Word { get; set; }
    public int Index { get; set; }
}
```

```csharp
BenchmarkRunner.Run<Benchmark>();
```

```console
| Method           | Mean      | Error    | StdDev   | Completed Work Items | Lock Contentions | Gen0      | Allocated   |
|----------------- |----------:|---------:|---------:|---------------------:|-----------------:|----------:|------------:|
| Parallel         |  71.59 ms | 0.797 ms | 0.746 ms |              11.0000 |                - |         - |     8.94 KB |
| ParallelClass    |  80.24 ms | 0.527 ms | 0.440 ms |              11.0000 |                - | 5000.0000 | 31258.96 KB |
| Sequential       | 336.07 ms | 6.390 ms | 5.977 ms |                    - |                - |         - |      1.3 KB |
| SequentialClass  | 356.85 ms | 6.360 ms | 5.949 ms |                    - |                - | 5000.0000 | 31251.28 KB |

// * Hints *
Outliers
  Benchmark.Parallel: Default -> 1 outlier  was  removed (76.16 ms)

// * Legends *
  Mean                 : Arithmetic mean of all measurements
  Error                : Half of 99.9% confidence interval
  StdDev               : Standard deviation of all measurements
  Completed Work Items : The number of work items that have been processed in ThreadPool (per single operation)
  Lock Contentions     : The number of times there was contention upon trying to take a Monitor's lock (per single operation)
  Allocated            : Allocated memory per single operation (managed only, inclusive, 1KB = 1024B)
  1 ms                 : 1 Millisecond (0.001 sec)

```

The `_hashSet.Contains` method in the predicate gives the query some "meat" and makes it worth parallelizing.

We could simplify the query slightly by using an anonymous type instead of the `IndexedWord` structure. However, this would degrade performance because anonymous types, being classes and therefore reference types, incur the cost of heap-based allocation and subsequent garbage collection.

The difference might not be enough to matter with sequential queries, but with parallel queries, favoring stack-based allocation can be quite advantageous. This is because stack-based allocation is highly parallelizable, as each thread has its own stack, whereas all threads must compete for the same heap — managed by a single memory manager and garbage collector.

## `Parallel`

The [↑ `Parallel`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.parallel) class provides support for parallel loops and regions.

## `AggregateException`

The [↑ `AggregateException`](https://learn.microsoft.com/en-us/dotnet/api/system.aggregateexception) class represents one or more errors that occur during application execution.
