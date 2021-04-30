# Covariance and contravariance

- [Covariance and contravariance](#covariance-and-contravariance)
  - [Arrays](#arrays)
    - [**Covariance**](#covariance)
  - [Generic type arguments](#generic-type-arguments)
    - [**Covariance**](#covariance-1)
    - [**Contravariance**](#contravariance)
  - [Delegates](#delegates)
    - [**Covarinace and contravariance**](#covarinace-and-contravariance)
  - [Links](#links)

Covariance enables implicit conversion of an array/delegate/generic argument of a more derived type to an array/delegate/generic argument of a less derived type.

## Arrays

### **Covariance**

Covariance for arrays:

```csharp
class Program
{
    static void Main(string[] args)
    {
        Rectangle[] array = { new Square() };
    }
}

class Rectangle
{
}

class Square : Rectangle
{
}
```

Object `array` is not type safe and should not be used. It is used here just for demonstration of covariance.

## Generic type arguments

Generic type variance is restricted to generic interfaces and generic delegates.

### **Covariance**

A covariant interface allows its methods to return more derived types than those specified in the interface:

```csharp
IEnumerable<object> list = new List<string>();
```

Another example which would not work without `out` keyword:

```csharp
class Program
{
    private delegate int Whatever(int value);

    static void Main(string[] args)
    {
        IA<Rectangle> a = new A<Square>();
    }
}

interface IA<out T>
{
    T Act();
}

class A<T> : IA<T>
{
    public T Act()
    {
        return default;
    }
}

class Rectangle
{
}

class Square : Rectangle
{
}
```

### **Contravariance**

A contravariant interface allows its methods to accept parameters of less derived types than those specified in the interface.

Here is an example which would not work without `in` keyword:

```csharp
class Program
{
    static void Main(string[] args)
    {
        IA<Square> a = new A<Rectangle>();
    }
}

interface IA<in T>
{
    void Act(T t);
}

class A<T> : IA<T>
{
    public void Act(T t)
    {

    }
}

class Rectangle
{
}

class Square : Rectangle
{
}
```

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

### **Covarinace and contravariance**

Covariance and contravariance support for *method groups* <sup>1</sup> allows for matching method signatures with [delegate](../api/system/delegate/delegate.md) types. This enables you to assign to delegates not only methods that have matching signatures, but also methods that return more derived types (covariance) or that accept parameters that have less derived types (contravariance) than that specified by the delegate type:

```csharp
using System;

class Program
{
    private delegate Rectangle MyDelegate(Square square);

    static void Main(string[] args)
    {
        MyDelegate d = D;
        Type output = d.Invoke(new Square()).GetType();
        Console.WriteLine($"Output type: {output}");
    }

    private static Square D(Rectangle rectangle)
    {
        Console.WriteLine($"Passed type: {rectangle.GetType()}");
        return new Square();
    }
}

class Rectangle
{
}

class Square : Rectangle
{
}
```

Output:

```output
Passed type: Square
Output type: Square
```

## Links

[↑ Variance in Generic Interfaces](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces)

[↑ Variance in Delegates](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/variance-in-delegates)

<hr>

<sup>1</sup> A **method group** is the name for a set of methods (that might be just one) — i.e. in theory the `ToString` method may have multiple overloads (plus any extension methods): `ToString()`,` ToString(string format)`, etc — hence `ToString` by itself is a "method group".

It is purely a compiler term for "I know what the method name is, but I don't know the signature"; it has no existence at runtime. AFAIK, the only use of a method-group by itself (no brackets etc) is during delegate construction.
