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

`Parser.cs`:

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

`DateParserBenchmarks.cs`:

```csharp
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Order;

[MemoryDiagnoser]
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

`Program.cs`

```csharp
using BenchmarkDotNet.Running;

BenchmarkRunner.Run<DateParserBenchmarks>();
```

### Run project

Build and run benchmark project:

```bash
dotnet build -c Release
dotnet PATH_TO_PROJECT_DLL_FILE
```

### Benchmark results

As a result of benchmark project run several files, with different extensions, containing results will be added under `BenchmarkDotNet.Artifacts` folder with contents like this:

| Method                              |       Mean |     Error |    StdDev | Ratio | Rank |  Gen 0 | Allocated |
| ----------------------------------- | ---------: | --------: | --------: | ----: | ---: | -----: | --------: |
| GetYearFromSpanWithManualConversion |   6.922 ns | 0.1398 ns | 0.1239 ns |  0.02 |    1 |      - |         - |
| GetYearFromSpan                     |  17.700 ns | 0.2097 ns | 0.1859 ns |  0.06 |    2 |      - |         - |
| GetYearFromSubstring                |  30.559 ns | 0.6229 ns | 0.5522 ns |  0.10 |    3 | 0.0051 |      32 B |
| GetYearFromSplit                    |  88.605 ns | 1.3126 ns | 1.2279 ns |  0.28 |    4 | 0.0254 |     160 B |
| GetYearFromDateTime                 | 315.677 ns | 5.3811 ns | 4.7702 ns |  1.00 |    5 |      - |         - |
