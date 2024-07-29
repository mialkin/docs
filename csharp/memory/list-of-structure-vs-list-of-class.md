# `List<Structure>` vs `List<Class>`

```csharp
BenchmarkRunner.Run<Benchmark>();

public class Structrure
{
    public int Id { get; set; }
}

public struct Class
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
// | CreateListOfStructures | 1,129.17 ms | 22.283 ms | 19.753 ms | 47000.0000 | 24000.0000 | 7000.0000 | 484.89 MB |
// | CreateListOfClasses    |    72.33 ms |  0.427 ms |  0.400 ms |  5857.1429 |  5857.1429 | 2285.7143 |    128 MB |

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
