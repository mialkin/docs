# Covariance and contravariance

Covariance enables implicit conversion of an array/delegate/generic argument of a more derived type to an array/delegate/generic argument of a less derived type.

## Arrays

Covariance for array:

```csharp
object[] array = new string[] { "one", "two", "three" };
```

Object `array` is not type safe and should not be used. It is used here just for demonstration of covariance.

## Generic type arguments

A covariant interface allows its methods to return more derived types than those specified in the interface:

```csharp
IEnumerable<object> list = new List<string>();
```

A contravariant interface allows its methods to accept parameters of less derived types than those specified in the interface.

Variance in generic interfaces is supported for reference types only. Value types do not support variance. For example, `IEnumerable<int>` cannot be implicitly converted to `IEnumerable<object>`, because integers are represented by a value type:

```csharp
IEnumerable<int> integers = new List<int>();
// The following statement generates a compiler error
// IEnumerable<Object> objects = integers;
```

It is also important to remember that classes that implement variant interfaces are still invariant. For example, although `List<T>` implements the covariant interface `IEnumerable<T>`, you cannot implicitly convert to `List<Object>`. This is illustrated in the following code example:

```csharp
// The following line generates a compiler error
// because classes are invariant.
// List<Object> list = new List<String>();

// You can use the interface object instead.
IEnumerable<Object> listObjects = new List<String>();
```

## Delegates

Covariance and contravariance support for method groups allows for matching method signatures with [delegate](../api/system/delegate/delegate.md) types. This enables you to assign to delegates not only methods that have matching signatures, but also methods that return more derived types (covariance) or that accept parameters that have less derived types (contravariance) than that specified by the delegate type.

## Links

[↑ Variance in Generic Interfaces (C#)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces)

[↑ Variance in Delegates (C#)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/variance-in-delegates)
