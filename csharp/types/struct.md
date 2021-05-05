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

Typically, you use structure types to design small data-centric types that provide little or no behavior. For example, .NET uses structure types to represent a number (both integer and real), a Boolean value, a Unicode character, a time instance. If you're focused on the behavior of a type, consider defining a class. Class types have *reference semantics*. That is, a variable of a class type contains a reference to an instance of the type, not the instance itself.

Because structure types have value semantics, we recommend you to define *immutable* structure types.

Struct's value types intialized with default values, reference types — with nulls:

```csharp
A a = new();

Console.WriteLine(a.X); // 0
Console.WriteLine(a.Y); // 0
Console.WriteLine(a.O == null); // True
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

If all instance fields of a structure type are accessible, you can also instantiate it without the new operator. In that case you must initialize all instance fields before the first use of the instance:

```csharp
A a;

Console.WriteLine(a.X); // Compile-time error: "Struct field 'a.X' might not be initialized before accessing"
a.X = 1; // Ok
a.Y = 2; // Compile-time error: "Local variable 'a' might not be initialized before accessing"

struct A
{
    public int X;
    public int Y { get; set; }
}
```

Important notes: https://docs.microsoft.com/en-us/dotnet/csharp/write-safe-efficient-code