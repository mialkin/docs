# Structure types, `Span<T>`

## Table of contents

- [Structure types, `Span<T>`](#structure-types-spant)
  - [Table of contents](#table-of-contents)
  - [Structure types](#structure-types)
    - [Initialization](#initialization)
  - [`Span<T>`, `ReadOnlySpan<T>`](#spant-readonlyspant)

## Structure types

A [↑ structure type](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/struct), or **struct type**, is a [value type](value-types.md) that can encapsulate data and related functionality.

You use the `struct` keyword to define a structure type:

```csharp
struct A
{
    public int X { get; set; }
    public int Y { get; set; }
    public object O { get; set; }
}
```

Typically, you use structure types to design small data-centric types that provide little or no behavior. For example, .NET uses structure types to represent a number, both integer and real, a Boolean value, a Unicode character, a time instance. If you're focused on the behavior of a type, consider defining a class. Class types have *reference semantics*. That is, a variable of a class type contains a reference to an instance of the type, not the instance itself.

Because structure types have value semantics, we recommend you to define *immutable* structure types.

### Initialization

Structures's value types initialized with default values, reference types — with `null`s:

```csharp
A a = new();

Console.WriteLine(a.X); // 0
Console.WriteLine(a.Y); // 0
Console.WriteLine(a.O == null); // True
```

Because structures are value types they are passed by value:

If all instance fields of a structure type are accessible, you can also instantiate it without the `new` operator. In that case you must initialize all instance fields before the first use of the instance:

```csharp
A a;

// Compile-time error: "Struct field 'a.X' might not be initialized before accessing"
// Console.WriteLine(a.X);

// Ok
a.X = 1;
a.Y = 2;

struct A
{
    public int X;
    public int Y;
}
```

## `Span<T>`, `ReadOnlySpan<T>`

The [↑ `Span<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.span-1) type provides a type-safe and memory-safe representation of a contiguous region of arbitrary memory.

`Span<T>` is a [↑ `ref struct`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/ref-struct) that is allocated on the stack rather than on the managed heap.

A `Span<T>` instance is often used to hold the elements of an array or a portion of an array. Unlike an array, however, a `Span<T>` instance can point to managed memory, native memory, or memory managed on the stack.

[↑ A brief overview of `Span<T>`](https://www.youtube.com/watch?v=byvoPD15CXs).

[↑ A Complete .NET Developer's Guide to Span with Stephen Toub](https://www.youtube.com/watch?v=5KdICNWOfEQ).

[↑ `Span<T>` on YouTube](https://www.youtube.com/results?search_query=nick+chapsas+span).

Example:

```csharp
BenchmarkRunner.Run<SpanBenchmark>();

[MemoryDiagnoser]
public class SpanBenchmark
{
    private const string DateAsText = "08 07 2021";

    [Benchmark]
    public (int day, int month, int year) DateWithSubstring()
    {
        string dayAsText = DateAsText.Substring(0, 2);
        string monthAsText = DateAsText.Substring(3, 2);
        string yearAsText = DateAsText.Substring(6);

        int day = int.Parse(dayAsText);
        int month = int.Parse(monthAsText);
        int year = int.Parse(yearAsText);

        return (day, month, year);
    }

    [Benchmark]
    public (int day, int month, int year) DateWithSpan()
    {
        ReadOnlySpan<char> dateAsSpan = DateAsText;

        ReadOnlySpan<char> dayAsText = dateAsSpan.Slice(0, 2);
        ReadOnlySpan<char> monthAsText = dateAsSpan.Slice(3, 2);
        ReadOnlySpan<char> yearAsText = dateAsSpan.Slice(6);

        int day = int.Parse(dayAsText);
        int month = int.Parse(monthAsText);
        int year = int.Parse(yearAsText);

        return (day, month, year);
    }
}
```

Output:

```console
| Method            | Mean     | Error    | StdDev   | Median   | Gen0   | Allocated |
|------------------ |---------:|---------:|---------:|---------:|-------:|----------:|
| DateWithSubstring | 60.00 ns | 1.217 ns | 2.345 ns | 58.93 ns | 0.0153 |      96 B |
| DateWithSpan      | 33.35 ns | 0.506 ns | 0.423 ns | 33.11 ns |      - |         - |

// * Legends *
  Mean      : Arithmetic mean of all measurements
  Error     : Half of 99.9% confidence interval
  StdDev    : Standard deviation of all measurements
  Median    : Value separating the higher half of all measurements (50th percentile)
  Gen0      : GC Generation 0 collects per 1000 operations
  Allocated : Allocated memory per single operation (managed only, inclusive, 1KB = 1024B)
  1 ns      : 1 Nanosecond (0.000000001 sec)
```
