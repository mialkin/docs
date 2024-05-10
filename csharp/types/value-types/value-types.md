# Value types, `ValueType`, boxing and unboxing

[↑ Value types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-types) and [reference types](../reference-types.md) are the two main categories of C# types.

A variable of a value type contains an instance of the type. This differs from a variable of a reference type, which contains a reference to an instance of the type.

If a value type contains a data member of a reference type, only the reference to the instance of the reference type is copied when a value type instance is copied. Both the copy and original value type instance have access to the same reference type instance.

A value type can be one of the two following kinds:

- a [structure type](struct.md), which encapsulates data and related functionality
- an [enumeration type](enumeration-types.md), which is defined by a set of named constants and represents a choice or a combination of choices

## Table of contents

- [Value types, `ValueType`, boxing and unboxing](#value-types-valuetype-boxing-and-unboxing)
  - [Table of contents](#table-of-contents)
  - [`ValueType`](#valuetype)
  - [Boxing and unboxing](#boxing-and-unboxing)
    - [Performance](#performance)

## `ValueType`

The [↑ `ValueType`](https://learn.microsoft.com/en-us/dotnet/api/system.valuetype) class provides the base class for value types.

```csharp
public abstract class ValueType
```

`ValueType` overrides the virtual methods from `Object` with more appropriate implementations for value types.

Although `ValueType` is the implicit base class for value types, you cannot create a class that inherits from `ValueType` directly. Instead, individual compilers provide a language keyword or construct, such as `struct` in C#, to support the creation of value types.

Aside from serving as the base class for value types in the .NET Framework, the `ValueType` structure is generally not used directly in code. However, it can be used as a parameter in method calls to restrict possible arguments to value types instead of all objects.

## Boxing and unboxing

The **boxing** is the process of converting a value type to the [`object`](../object.md) type or to any [↑ interface type](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interface) implemented by this value type.

When the CLR boxes a value type, it wraps the value inside a `System.Object` instance and stores it on the managed heap. Unboxing extracts the value type from the object.

Boxing is implicit, unboxing is explicit.

The concept of boxing and unboxing underlies the C# unified view of the type system in which a value of any type can be treated as an object.

### Performance

[↑ Boxing and unboxing](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/boxing-and-unboxing) are computationally expensive processes. When a value type is boxed, an entirely new object must be created. This can take up to 20 times longer than a simple reference assignment. When unboxing, the casting process can take four times as long as an assignment.

In generics, such as `List<T>`, value types are still stored on the heap. The difference is that, internally, a `List<int>` makes a single array of integers, and can store the numbers directly. With `ArrayList` you end up storing an array of references to boxed integer values.
