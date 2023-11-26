# Value types

**Value types** and [**reference types**](reference-types.md) are the two main categories of C# types.

A variable of a value type contains an instance of the type. This differs from a variable of a reference type, which contains a reference to an instance of the type.

If a value type contains a data member of a reference type, only the reference to the instance of the reference type is copied when a value-type instance is copied. Both the copy and original value-type instance have access to the same reference-type instance:

```csharp
using System;

A a1 = new A
{
    X = 5,
    Y = new B {Title = "Spring"}
};

A a2 = a1;

Console.WriteLine("X1: " + a2.X);        // 5
Console.WriteLine("Y1: " + a2.Y.Title);  // Spring

a2.X = 10;
a2.Y.Title = "Summer";

Console.WriteLine("X1: " + a1.X);        // 5
Console.WriteLine("Y1: " + a1.Y.Title);  // Summer
Console.WriteLine("X2: " + a2.X);        // 10
Console.WriteLine("Y2: " + a2.Y.Title);  // Summer

struct A
{
    public int X;
    public B Y;
}

class B
{
    public string Title { get; set; }
}
```

Output:

```console
X1: 5
Y1: Spring
X1: 5
Y1: Summer
X2: 10
Y2: Summer
```

If in the code above `class B` would be replaced with `struct B`, then we would get the following output:

```console
X1: 5
Y1: Spring
X1: 5
Y1: Spring
X2: 10
Y2: Summer
```

A value type can be one of the two following kinds:

- a *structure type*, which encapsulates data and related functionality
- an *enumeration type*, which is defined by a set of named constants and represents a choice or a combination of choices

A nullable value type `T?` represents all values of its underlying value type `T` and an additional `null` value.

## Links

[↑ Value types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-types).
