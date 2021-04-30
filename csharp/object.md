# Object

- [Object](#object)
  - [Equals](#equals)
  - [Finalize](#finalize)
  - [GetHashCode](#gethashcode)
  - [GetType](#gettype)
  - [MemberwiseClone](#memberwiseclone)
  - [ReferenceEquals](#referenceequals)
  - [ToString](#tostring)

## Equals

Determines whether two object instances are equal.

https://docs.microsoft.com/en-us/dotnet/api/system.object.equals
https://stackoverflow.com/questions/814878/c-sharp-difference-between-and-equals

## Finalize

https://docs.microsoft.com/en-us/dotnet/api/system.object.finalize

## GetHashCode

https://docs.microsoft.com/en-us/dotnet/api/system.object.gethashcode

## GetType

https://docs.microsoft.com/en-us/dotnet/api/system.object.gettype

## MemberwiseClone

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

[Object.MemberwiseClone Method](https://docs.microsoft.com/en-us/dotnet/api/system.object.memberwiseclone)

## ReferenceEquals

Determines whether the specified Object instances are the same instance.

```csharp
public static bool ReferenceEquals (object objA, object objB);
```

Returns `true` if `objA` is the same instance as `objB` or if both are `null`; otherwise, `false`:

```csharp
using System.Runtime.CompilerServices;

namespace System
{
    public partial class Object
    {
        public static bool ReferenceEquals(object? objA, object? objB)
        {
            return objA == objB;
        }
    }
}
```

Unlike the [Equals](Equals.md) method and the equality operator, the `ReferenceEquals` method cannot be overridden. Because of this, if you want to test two object references for equality and you are unsure about the implementation of the Equals method, you can call the `ReferenceEquals` method.

[↑ Object.ReferenceEquals(Object, Object) Method](https://docs.microsoft.com/en-us/dotnet/api/system.object.referenceequals)

## ToString

```csharp
namespace System
{
    public partial class Object
    {
        // Returns a String which represents the object instance.  The default
        // for an object is to return the fully qualified name of the class.
        public virtual string? ToString()
        {
            return GetType().ToString();
        }
    }
}
```

