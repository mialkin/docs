# `List<Structure>` vs `List<Class>`

```csharp
BenchmarkRunner.Run<Benchmark>();

public class Class
{
    public int Id { get; set; }
}

public struct Structrure
{
    public int Id { get; set; }
}

[MemoryDiagnoser]
public class Benchmark
{
    [Benchmark]
    public void CreateListOfStructures()
    {
        var list = new List<Structrure>();
        for (var i = 0; i < 10_000_000; i++)
            list.Add(new Structrure { Id = i });
    }

    [Benchmark]
    public void CreateListOfClasses()
    {
        var list = new List<Class>();
        for (var i = 0; i < 10_000_000; i++)
            list.Add(new Class { Id = i });
    }
}

// Output:
// | Method                 | Mean        | Error     | StdDev    | Gen0       | Gen1       | Gen2      | Allocated |
// |----------------------- |------------:|----------:|----------:|-----------:|-----------:|----------:|----------:|
// | CreateListOfStructures |    71.91 ms |  0.538 ms |  0.503 ms |  6571.4286 |  6571.4286 | 1714.2857 |    128 MB |
// | CreateListOfClasses    | 1,140.53 ms | 21.701 ms | 20.299 ms | 47000.0000 | 24000.0000 | 7000.0000 | 484.89 MB |

// * Legends *
//  Mean      : Arithmetic mean of all measurements
//  Error     : Half of 99.9% confidence interval
//  StdDev    : Standard deviation of all measurements
//  Gen0      : GC Generation 0 collects per 1000 operations
//  Gen1      : GC Generation 1 collects per 1000 operations
//  Gen2      : GC Generation 2 collects per 1000 operations
//  Allocated : Allocated memory per single operation (managed only, inclusive, 1KB = 1024B)
//  1 ms      : 1 Millisecond (0.001 sec)
```

```bash
dotnet run --configuration=Release
```

[↑ Smallest .Net ref type is 12 bytes (or why you should consider using value types)](https://theburningmonk.com/2015/07/smallest-net-ref-type-is-12-bytes-or-why-you-should-consider-using-value-types/).

Using arrays instead of lists improves overall situation with memory consumption:

```csharp
[MemoryDiagnoser]
public class Benchmark
{
    [Benchmark]
    public void CreateListOfStructures()
    {
        var array = new Structrure[10_000_000];
        for (var i = 0; i < 10_000_000; i++)
            array[i] = new Structrure { Id = i };
    }

    [Benchmark]
    public void CreateListOfClasses()
    {
        var array = new Class[10_000_000];
        for (var i = 0; i < 10_000_000; i++)
            array[i] = new Class { Id = i };
    }
}

// Output:
// | Method                  | Mean      | Error    | StdDev   | Gen0       | Gen1       | Gen2      | Allocated |
// |------------------------ |----------:|---------:|---------:|-----------:|-----------:|----------:|----------:|
// | CreateArrayOfStructures |  21.99 ms | 0.439 ms | 0.410 ms |   968.7500 |   968.7500 |  968.7500 |  38.15 MB |
// | CreateArrayOfClasses    | 977.04 ms | 8.623 ms | 7.645 ms | 44000.0000 | 22000.0000 | 6000.0000 | 305.18 MB |
```
