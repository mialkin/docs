# Covariance and contravariance

Covariance enables implicit conversion of an array/delegate/generic argument of a more derived type to an array/delegate/generic argument of a less derived type.

## Arrays

Covariance for array:

```csharp
object[] array = new string[] { "one", "two", "three" };
```

Object `array` is not type safe and should not be used. It is used here just for demonstration of covariance.

## Generic type arguments

Covariance for generic type argument:

```csharp
IEnumerable<object> list = new List<string>();
```

## Delegates

Covariance and contravariance support for method groups allows for matching method signatures with delegate types. This enables you to assign to delegates not only methods that have matching signatures, but also methods that return more derived types (covariance) or that accept parameters that have less derived types (contravariance) than that specified by the delegate type.

## Links

[Variance in Generic Interfaces (C#)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces)
