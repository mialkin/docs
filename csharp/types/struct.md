# Structure types

- [Structure types](#structure-types)
  - [0](#0)
  - [1 Copy of field/parameter of reference type](#1-copy-of-fieldparameter-of-reference-type)
  - [Instantiation without `new`](#instantiation-without-new)

A **structure type** (or **struct type**) is a value type that can encapsulate data and related functionality. You use the `struct` keyword to define a structure type:

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

## 0

Struct's value types intialized with default values, reference types — with nulls:

```csharp
A a = new();

Console.WriteLine(a.X); // 0
Console.WriteLine(a.Y); // 0
Console.WriteLine(a.O == null); // True
```

## 1 Copy of field/parameter of reference type

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
    b.O = "2"; // Doesn't affect original value because b.O here is another variable

    return b;
}
```

Another example:

```csharp
using System;

A a = new A
{
    Id = 1,
    B = new B
    {
        Id = 1,
        C = new C
        {
            Id = 1
        }
    }
};

Console.WriteLine($"{a.Id}, {a.B.Id}, {a.B.C.Id}"); // 1, 1, 1

struct A
{
    public int Id { get; set; }

    public B B { get; set; }
}

class B
{
    public int Id { get; set; }

    public C C { get; set; }
}

class C
{
    public int Id { get; set; }
}
```

Adding this code to the end outputs `1, 2, 2`:

```csharp
M(a);
Console.WriteLine($"{a.Id}, {a.B.Id}, {a.B.C.Id}"); // 1, 2, 2

void M(A a)
{
    a.Id = 2;
    a.B.Id = 2;
    a.B.C = new C
    {
        Id = 2
    };
}
```

Adding this code to the end ouputs `1, 1, 1`:

```csharp
M(a);
Console.WriteLine($"{a.Id}, {a.B.Id}, {a.B.C.Id}");

void M(A a)
{
    a.Id = 2;
    a.B = new B
    {
        Id = 2,
        C = new C
        {
            Id = 2
        }
    };
}
```

## Instantiation without `new`

If all instance fields of a structure type are accessible, you can also instantiate it without the `new` operator. In that case you must initialize all instance fields before the first use of the instance:

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
Can structs contain fields of reference types? https://stackoverflow.com/questions/945664/can-structs-contain-fields-of-reference-types