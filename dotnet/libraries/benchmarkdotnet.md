# BenchmarkDotNet

[↑ BenchmarkDotNet](https://github.com/dotnet/BenchmarkDotNet) is an open source .NET library that helps to transform methods into benchmarks, track their performance, and share reproducible measurement experiments.

BenchmarkDotNet works only with console applications. It does not support any other kind of application like ASP.NET.

> Benchmarking is a process of running a test that measures the performance of a software

Under the hood, BenchmarkDotNet performs a lot of magic that guarantees reliable and precise results thanks to the [↑ perfolizer](https://github.com/AndreyAkinshin/perfolizer) statistical engine. BenchmarkDotNet protects you from popular benchmarking mistakes and warns you if something is wrong with your benchmark design or obtained measurements. The results are presented in a user-friendly form that highlights all the important facts about your experiment.

## Table of contents

- [BenchmarkDotNet](#benchmarkdotnet)
  - [Table of contents](#table-of-contents)
  - [Good practices](#good-practices)
  - [Benchmark project](#benchmark-project)
    - [Create new project](#create-new-project)
    - [Run project](#run-project)
    - [Benchmark results](#benchmark-results)

## Good practices

[↑ Good practices](https://benchmarkdotnet.org/articles/guides/good-practices.html).

## Benchmark project

### Create new project

Create a new console application and add NuGet package to it:

```bash
dotnet add package BenchmarkDotNet
```

Create `DateParser.cs` file:

```csharp
public class DateParser
{
    public int GetYearFromDateTime(string dateTimeAsString)
    {
        var dateTime = DateTime.Parse(dateTimeAsString);
        return dateTime.Year;
    }

    public int GetYearFromSplit(string dateTimeAsString)
    {
        var splitOnHyphen = dateTimeAsString.Split('-');
        return int.Parse(splitOnHyphen[0]);
    }

    public int GetYearFromSubstring(string dateTimeAsString)
    {
        var indexOfHyphen = dateTimeAsString.IndexOf('-');
        return int.Parse(dateTimeAsString.Substring(0, indexOfHyphen));
    }

    public int GetYearFromSpan(ReadOnlySpan<char> dateTimeAsSpan)
    {
        var indexOfHyphen = dateTimeAsSpan.IndexOf('-');
        return int.Parse(dateTimeAsSpan.Slice(0, indexOfHyphen));
    }

    public int GetYearFromSpanWithManualConversion(ReadOnlySpan<char> dateTimeAsSpan)
    {
        var indexOfHyphen = dateTimeAsSpan.IndexOf('-');
        var yearAsSpan = dateTimeAsSpan.Slice(0, indexOfHyphen);

        var temp = 0;
        for (int i = 0; i < yearAsSpan.Length; i++)
        {
            temp = temp * 10 + (yearAsSpan[i] - '0');
        }

        return temp;
    }
}
```

Create `DateParserBenchmarks.cs` file:

```csharp
[MemoryDiagnoser]
[ThreadingDiagnoser]
[Orderer(SummaryOrderPolicy.FastestToSlowest)]
[RankColumn]
public class DateParserBenchmarks
{
    private const string DateTime = "2019-12-13T16:33:06Z";
    private static readonly DateParser Parser = new DateParser();

    [Benchmark(Baseline = true)]
    public void GetYearFromDateTime()
    {
        Parser.GetYearFromDateTime(DateTime);
    }

    [Benchmark]
    public void GetYearFromSplit()
    {
        Parser.GetYearFromSplit(DateTime);
    }

    [Benchmark]
    public void GetYearFromSubstring()
    {
        Parser.GetYearFromSubstring(DateTime);
    }

    [Benchmark]
    public void GetYearFromSpan()
    {
        Parser.GetYearFromSpan(DateTime);
    }

    [Benchmark]
    public void GetYearFromSpanWithManualConversion()
    {
        Parser.GetYearFromSpanWithManualConversion(DateTime);
    }
}
```

Add to `Program.cs` file:

```csharp
using BenchmarkDotNet.Running;

BenchmarkRunner.Run<DateParserBenchmarks>();
```

### Run project

```bash
dotnet run --configuration Release
```

or:

```bash
dotnet build --configuration Release
dotnet bin/Release/net8.0/ConsoleApp1.dll 
```

### Benchmark results

As a result of benchmark project run several files will be added under `BenchmarkDotNet.Artifacts` folder with contents like below. File will have different extensions.

```console
| Method                              | Mean       | Error     | StdDev    | Ratio | Rank | Completed Work Items | Lock Contentions | Gen0   | Allocated | Alloc Ratio |
|------------------------------------ |-----------:|----------:|----------:|------:|-----:|---------------------:|-----------------:|-------:|----------:|------------:|
| GetYearFromSpanWithManualConversion |   7.036 ns | 0.1571 ns | 0.2538 ns |  0.03 |    1 |                    - |                - |      - |         - |          NA |
| GetYearFromSpan                     |  15.826 ns | 0.1822 ns | 0.1615 ns |  0.06 |    2 |                    - |                - |      - |         - |          NA |
| GetYearFromSubstring                |  25.734 ns | 0.5330 ns | 0.6545 ns |  0.09 |    3 |                    - |                - | 0.0051 |      32 B |          NA |
| GetYearFromSplit                    |  86.475 ns | 1.5104 ns | 1.4129 ns |  0.31 |    4 |                    - |                - | 0.0254 |     160 B |          NA |
| GetYearFromDateTime                 | 276.873 ns | 5.5410 ns | 5.4420 ns |  1.00 |    5 |                    - |                - |      - |         - |          NA |

// * Legends *
  Mean                 : Arithmetic mean of all measurements
  Error                : Half of 99.9% confidence interval
  StdDev               : Standard deviation of all measurements
  Ratio                : Mean of the ratio distribution ([Current]/[Baseline])
  Rank                 : Relative position of current benchmark mean among all benchmarks (Arabic style)
  Completed Work Items : The number of work items that have been processed in ThreadPool (per single operation)
  Lock Contentions     : The number of times there was contention upon trying to take a Monitor's lock (per single operation)
  Gen0                 : GC Generation 0 collects per 1000 operations
  Allocated            : Allocated memory per single operation (managed only, inclusive, 1KB = 1024B)
  Alloc Ratio          : Allocated memory ratio distribution ([Current]/[Baseline])
  1 ns                 : 1 Nanosecond (0.000000001 sec)
```
