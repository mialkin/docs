# Value types

**Value types** and [**reference types**](reference%20types.md) are the two main categories of C# types. A variable of a value type contains an instance of the type. This differs from a variable of a reference type, which contains a reference to an instance of the type.

If a value type contains a data member of a reference type, only the reference to the instance of the reference type is copied when a value-type instance is copied. Both the copy and original value-type instance have access to the same reference-type instance:

```csharp
using System;

A a1 = new A
{
    X = 5,
    Y = new B {Title = "Spring"}
};

A a2 = a1;

Console.WriteLine("X: " + a2.X);        // 5
Console.WriteLine("Y: " + a2.Y.Title);  // Spring

a2.X = 10;
a2.Y.Title = "Summer";

Console.WriteLine("X: " + a1.X);        // 5
Console.WriteLine("X: " + a1.Y.Title);  // Summer
Console.WriteLine("X: " + a2.X);        // 10
Console.WriteLine("X: " + a2.Y.Title);  // Summer

struct A
{
    public int X;
    public B Y { get; set; }

    public A(int x, B y)
    {
        X = x;
        Y = y;
    }
}

class B
{
    public string Title { get; set; }
}
```
