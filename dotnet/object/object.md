# Object

## Methods

### Object.MemberwiseClone

The `MemberwiseClone` method creates a shallow copy by creating a new object, and then copying the nonstatic fields of the current object to the new object. If a field is a value type, a bit-by-bit copy of the field is performed. If a field is a reference type, the reference is copied but the referred object is not; therefore, the original object and its clone refer to the same object.

```csharp
protected object MemberwiseClone ();
```

Example of usage:

```csharp
using System;

class Program
{
    static void Main()
    {
        var a = new A { Num = 7, Str = "hello", B = new B { Sum = 10 } };
        var b = (A) a.Clone();

        Print(a);
        Print(b);

        a.Num = 8;
        a.Str = "bye";
        a.B.Sum = 11;

        Console.WriteLine("----------------");
        Print(a);
        Print(b);
    }

    private static void Print(A a)
    {
        Console.WriteLine($"{a.Num}, {a.Str}, {a.B.Sum}");
    }
}

class A : ICloneable
{
    public int Num;

    public string Str { get; set; }

    public B B { get; set; }

    public object Clone()
    {
        return MemberwiseClone();
    }
}

class B
{
    public int Sum { get; set; }
}
```

Output:

```output
7, hello, 10
7, hello, 10
----------------
8, bye, 11
7, hello, 11
```

## See also

[Object.MemberwiseClone Method](https://docs.microsoft.com/en-us/dotnet/api/system.object.memberwiseclone)

[Static Constructors](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/static-constructors)
