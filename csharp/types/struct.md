# Structure types

A **structure type** is a value type that can encapsulate data and related functionality. You use the `struct` keyword to define a structure type:

```csharp
struct A
{
    public int X { get; set; }
    public int Y { get; set; }
    public object O { get; set; }
}
```

Struct's value types intialized with default values, reference types — with nulls:

```csharp
A a = new();

Console.WriteLine(a.X); // 0
Console.WriteLine(a.Y); // 0
Console.WriteLine(a.O == null); // 0
```

Because structs are value types they are passed by value:

```csharp
A a = new A {X = 1, Y = 1, O = "1"};

A result = M(a);

Console.WriteLine(a.X); // 1
Console.WriteLine(a.Y); // 1
Console.WriteLine(a.O); // 2

Console.WriteLine(result.X); // 2
Console.WriteLine(result.Y); // 2
Console.WriteLine(result.O); // 2

A M(A b)
{
    b.X = 2;
    b.Y = 2;
    b.O = "2"; // Doesn't affect original value

    return b;
}
```

Important notes: https://docs.microsoft.com/en-us/dotnet/csharp/write-safe-efficient-code