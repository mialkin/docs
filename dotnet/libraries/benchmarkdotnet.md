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

Build and run benchmark project:

```bash
dotnet build --configuration Release
dotnet dotnet bin/Release/net8.0/ConsoleApp1.dll 
```

### Benchmark results

As a result of benchmark project run several files will be added under `BenchmarkDotNet.Artifacts` folder with contents like below. File will have different extensions.

| Method                              | Mean       | Error     | StdDev    | Ratio | Rank | Gen0   | Allocated | Alloc Ratio |
|------------------------------------ |-----------:|----------:|----------:|------:|-----:|-------:|----------:|------------:|
| GetYearFromSpanWithManualConversion |   6.861 ns | 0.0663 ns | 0.0588 ns |  0.02 |    1 |      - |         - |          NA |
| GetYearFromSpan                     |  16.067 ns | 0.1350 ns | 0.1197 ns |  0.06 |    2 |      - |         - |          NA |
| GetYearFromSubstring                |  25.199 ns | 0.3182 ns | 0.2977 ns |  0.09 |    3 | 0.0051 |      32 B |          NA |
| GetYearFromSplit                    |  81.191 ns | 1.6119 ns | 1.4289 ns |  0.30 |    4 | 0.0254 |     160 B |          NA |
| GetYearFromDateTime                 | 274.407 ns | 4.7622 ns | 5.0955 ns |  1.00 |    5 |      - |         - |          NA |
