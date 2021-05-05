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

A original = new A
{
    Code = 1,
    B = new B
    {
        Code = 1,
        C = new C
        {
            Code = 1
        }
    }
};

Console.WriteLine($"{original.Code}, {original.B.Code}, {original.B.C.Code}"); // 1, 1, 1

A copy = original.Clone();
Console.WriteLine($"{copy.Code}, {copy.B.Code}, {copy.B.C.Code}"); // 1, 1, 1

copy.Code = 2;
copy.B.Code = 3;
Console.WriteLine($"{original.Code}, {copy.Code}, {original.B.Code}, {copy.B.Code}"); // 1, 2, 3, 3

copy.B.C.Code = 4;
Console.WriteLine($"{original.B.C.Code}, {copy.B.C.Code}"); // 4, 4

class A
{
    public int Code { get; set; }

    public B B { get; set; }

    public A Clone()
    {
        return (A) MemberwiseClone();
    }
}

class B
{
    public int Code { get; set; }
    public C C { get; set; }
}

class C
{
    public int Code { get; set; }
}
```

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
