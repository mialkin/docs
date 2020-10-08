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

This "covariance" does not work:

```csharp
// List<object> list = new List<string>();
```

This "contravariance" does not work too:

```csharp
// IEnumerable<string> list = new List<object>();
```

## Delegates

Covariance and contravariance support for method groups allows for matching method signatures with delegate types. This enables you to assign to delegates not only methods that have matching signatures, but also methods that return more derived types (covariance) or that accept parameters that have less derived types (contravariance) than that specified by the delegate type. 