# Structure types, `Span<T>`

## Table of contents

- [Structure types, `Span<T>`](#structure-types-spant)
  - [Table of contents](#table-of-contents)
  - [Structure types](#structure-types)
    - [Initialization](#initialization)
  - [`Span<T>`](#spant)

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

## `Span<T>`

`Span<T>` type provides a type-safe and memory-safe representation of a contiguous region of arbitrary memory.

`Span<T>` is a [↑ `ref struct`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/ref-struct) that is allocated on the stack rather than on the managed heap.

A `Span<T>` instance is often used to hold the elements of an array or a portion of an array. Unlike an array, however, a `Span<T>` instance can point to managed memory, native memory, or memory managed on the stack.

[↑ A brief overview of `Span<T>`](https://www.youtube.com/watch?v=byvoPD15CXs).

[↑ `Span<T>` structure](https://learn.microsoft.com/en-us/dotnet/api/system.span-1).

[↑ `Span<T>` on YouTube](https://www.youtube.com/results?search_query=nick+chapsas+span).
