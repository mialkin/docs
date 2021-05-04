# Object

- [Object](#object)
  - [Equals(Object)](#equalsobject)
  - [Equals(Object, Object)](#equalsobject-object)
  - [Finalize()](#finalize)
  - [GetHashCode()](#gethashcode)
  - [GetType()](#gettype)
  - [MemberwiseClone()](#memberwiseclone)
  - [ReferenceEquals(Object, Object)](#referenceequalsobject-object)
  - [ToString()](#tostring)

The `object` type is an alias for `System.Object` in .NET. In the unified type system of C#, all types, predefined and user-defined, reference types and value types, inherit directly or indirectly from `System.Object`.

## Equals(Object)

Determines whether two object instances are equal.

https://docs.microsoft.com/en-us/dotnet/api/system.object.equals
https://stackoverflow.com/questions/814878/c-sharp-difference-between-and-equals

## Equals(Object, Object)

## Finalize()

https://docs.microsoft.com/en-us/dotnet/api/system.object.finalize

## GetHashCode()

https://docs.microsoft.com/en-us/dotnet/api/system.object.gethashcode

## GetType()

https://docs.microsoft.com/en-us/dotnet/api/system.object.gettype

## MemberwiseClone()

Creates a shallow copy of the current `Object`.

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

## ReferenceEquals(Object, Object)

Determines whether the specified `Object` instances are the same instance.

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

[Object.ReferenceEquals(Object, Object) Method ↑](https://docs.microsoft.com/en-us/dotnet/api/system.object.referenceequals)

## ToString()

Returns a string that represents the current object.

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
