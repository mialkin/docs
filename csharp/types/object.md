# `object`

The `object` keyword is an alias for [↑ `System.Object`](https://learn.microsoft.com/en-us/dotnet/api/system.object) type; therefore, `System.Object` and `object` are equivalent.

In C# type system all types, predefined and user-defined, reference types and value types, inherit directly or indirectly from `System.Object`.

An **object** is a block of memory that has been allocated and configured according to the definition of its type.

An **object** is an instance of a class.

## Table of contents

- [`object`](#object)
  - [Table of contents](#table-of-contents)
  - [Methods](#methods)
    - [`Equals`](#equals)
    - [`Finalize`](#finalize)
    - [`GetHashCode`](#gethashcode)
    - [`GetType`](#gettype)
    - [`MemberwiseClone`](#memberwiseclone)
    - [`ReferenceEquals`](#referenceequals)
    - [`ToString`](#tostring)

## Methods

### `Equals`

The [↑ `Equals`](https://learn.microsoft.com/en-us/dotnet/fundamentals/runtime-libraries/system-object-equals) method determines whether two object instances are equal.

[↑ C# difference between `==` and `Equals()`](https://stackoverflow.com/questions/814878/c-sharp-difference-between-and-equals).

### `Finalize`

The [↑ `Finalize`](https://learn.microsoft.com/en-us/dotnet/fundamentals/runtime-libraries/system-object-finalize) method allows an object to try to free resources and perform other cleanup operations before it is reclaimed by garbage collection.

### `GetHashCode`

The [↑ `GetHashCode`](https://learn.microsoft.com/en-us/dotnet/fundamentals/runtime-libraries/system-object-gethashcode) method provides a hash code for algorithms that need quick checks of object equality.

```csharp
public virtual int GetHashCode() => RuntimeHelpers.GetHashCode(this);
```

A hash code is a numeric value that is used to insert and identify an object in a hash-based collection, such as the `Dictionary<TKey,TValue>` class, the `Hashtable` class, or a type derived from the [↑ `DictionaryBase`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.dictionarybase) class.

Two objects that are equal return hash codes that are equal. However, the reverse is not true: equal hash codes do not imply object equality, because different (unequal) objects can have identical hash codes.

If you override the `GetHashCode` method, you should also override `Equals`, and vice versa. If your overridden `Equals` method returns `true` when two objects are tested for equality, your overridden `GetHashCode` method must return the same value for the two objects.

The `GetHashCode` method can be overridden by a derived type. If `GetHashCode` is not overridden, hash codes for reference types are computed by calling the`Object.GetHashCode` method of the base class, which computes a hash code based on an object's reference. In other words, two objects for which the [`ReferenceEquals`](#referenceequals) method returns true have identical hash codes.

If value types do not override `GetHashCode`, the [↑ `ValueType.GetHashCode`](https://learn.microsoft.com/en-us/dotnet/api/system.valuetype.gethashcode) method of the base class uses reflection to compute the hash code based on the values of the type's fields. In other words, value types whose fields have equal values have equal hash codes.

### `GetType`

The [↑ `GetType`](https://learn.microsoft.com/en-us/dotnet/api/system.object.gettype) method gets the [↑ type declaration](https://learn.microsoft.com/en-us/dotnet/api/system.type) of the current instance.

### `MemberwiseClone`

Creates a shallow copy of the current `object`.

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

### `ReferenceEquals`

The [↑ `ReferenceEquals`](https://learn.microsoft.com/en-us/dotnet/api/system.object.referenceequals) method determines whether the specified `object` instances are the same instance.

```csharp
public static bool ReferenceEquals(object? objA, object? objB) => objA == objB;
```

Unlike the [`Equals`](#equals) method and the equality operator, the `ReferenceEquals` method cannot be overridden. Because of this, if you want to test two object references for equality and you are unsure about the implementation of the `Equals` method, you can call the `ReferenceEquals` method.

### `ToString`

The [↑ `ToString`](https://learn.microsoft.com/en-us/dotnet/fundamentals/runtime-libraries/system-object-tostring) method returns a string that represents the current object.

```csharp
public virtual string? ToString() => this.GetType().ToString();
```
