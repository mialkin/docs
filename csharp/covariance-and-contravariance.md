# Variance, invariance, covariance, contravariance

## Table of contents

- [Variance, invariance, covariance, contravariance](#variance-invariance-covariance-contravariance)
  - [Table of contents](#table-of-contents)
  - [Invariance](#invariance)
  - [Covariance](#covariance)
    - [Arrays](#arrays)
      - [Example 1](#example-1)
      - [Example 2](#example-2)
    - [Interfaces](#interfaces)
      - [Example 1](#example-1-1)
      - [Example 2](#example-2-1)
      - [Example 3](#example-3)
    - [Delegates](#delegates)
  - [Example 1](#example-1-2)
  - [Contravariance](#contravariance)
    - [Interfaces](#interfaces-1)
      - [Example 1](#example-1-3)
    - [Delegates](#delegates-1)
      - [Example 1](#example-1-4)
  - [Variance](#variance)
  - [Delegates](#delegates-2)
    - [Covariance and contravariance](#covariance-and-contravariance)
  - [Links](#links)

## Invariance

An **invariance** is a feature that allows to only use the type originally specified in the context.

```csharp
IProvider<Derived> provider = new Provider<Derived>();

interface IProvider<T>
{
    T Get();
    void Update(T item);
}

class Provider<T> : IProvider<T> where T : new()
{
    public T Get()
    {
        return new T();
    }

    public void Update(T item)
    {
    }
}

class Base
{
}

class Derived : Base
{
}
```

In the example above both of these lines would not compile:

```csharp
IProvider<Base> provider = new Provider<Derived>(); // Cannot convert initializer type 'Provider<Derived>' to target type 'IProvider<Base>'
IProvider<Derived> provider2 = new Provider<Base>(); // Cannot convert initializer type 'Provider<Base>' to target type 'IProvider<Derived>'
```

## Covariance

A **covariance** is a feature that allows to use a more derived type in the context that requires a less derived type.

### Arrays

#### Example 1

```csharp
Base[] array =
{
    new(),
    new Derived()
};

class Base
{
}

class Derived : Base
{
}
```

#### Example 2

Object array is not type safe and should not be used; it is used here just for demonstration of covariance:

```csharp
object[] array = new string[3] { "one", "two", "three" };
```

More succinct version:

```csharp
object[] array = { "one", "two", "three" };
```

### Interfaces

A covariant interface allows its methods to return instances of less derived types than those specified in the interface.

#### Example 1

```csharp
IEnumerable<Base> list = new List<Derived>(); 
```

#### Example 2

```csharp
IProvider<Base> provider = new Provider<Derived>();

interface IProvider<out T>
{
    T Get();
    // void Update(T item); // Invalid variance: covariant type parameter 'T' is used in contravariant position. Parameter must be input-safe
}

class Provider<T> : IProvider<T> where T : new()
{
    public T Get()
    {
        return new T();
    }
}

class Base
{
}

class Derived : Base
{
}
```

#### Example 3

```csharp
var list = new List<Derived>
{
    new()
};

Base.PrintBases(list);

class Base
{
    public static void PrintBases(IEnumerable<Base> bases)
    {
        foreach (var @base in bases)
            Console.WriteLine(@base.GetType());
    }
}

class Derived : Base
{
}
```

### Delegates

A covariance in delegates means that delegates can be used with methods that have return types that are derived from the return type in the delegate signature.

## Example 1

```csharp
Print print = PrintLocal;

Derived PrintLocal()
{
    return new Derived();
}

delegate Base Print();

class Base
{
}

class Derived : Base
{
}
```

## Contravariance

A **contravariance** is a feature that allows to use a less derived type in the context that requires a more derived type.

### Interfaces

A contravariant interface allows its methods to accept parameters of less derived types than those specified in the interface.

#### Example 1

```csharp
IProvider<Derived> provider = new Provider<Base>();

interface IProvider<in T>
{
    // T Get(); // Invalid variance: contravariant type parameter 'T' is used in covariant position. Method return type must be output-safe
    void Update(T item);
}

class Provider<T> : IProvider<T> where T : new()
{
    public void Update(T item)
    {
    }
}

class Base
{
}

class Derived : Base
{
}
```

### Delegates

A contravariance in delegates means that delegates can be used with methods that have parameters whose types are base types of the delegate signature parameter type. With contravariance, you can use one event handler instead of separate handlers.

#### Example 1

```csharp
Print print = PrintLocal;

print(new Derived());

void PrintLocal(Base input)
{
    Console.WriteLine(input.GetType());
}

delegate void Print(Derived input);

class Base
{
}

class Derived : Base
{
}
```

## Variance

*Covariance* and *contravariance* are collectively referred to as **variance**.

Variance in generic interfaces is supported for reference types only. Value types do not support variance. For example, `IEnumerable<int>` cannot be implicitly converted to `IEnumerable<object>`, because integers are represented by a value type:

```csharp
IEnumerable<int> list = new List<int>();
// The following statement generates a compiler error
// IEnumerable<object> list = new List<int>();
```

It is also important to remember that classes that implement variant interfaces are still invariant. For example, although `List<T>` implements the covariant interface `IEnumerable<T>`, you cannot implicitly convert to `List<object>`. This is illustrated in the following code example:

```csharp
IEnumerable<object> listObjects = new List<string>();

// The following line generates a compiler error because classes are invariant
// List<object> list = new List<string>();
```

## Delegates

### Covariance and contravariance

Covariance and contravariance support for *method groups* <sup>1</sup> allows for matching method signatures with [delegate](types/delegate.md) types. This enables you to assign to delegates not only methods that have matching signatures, but also methods that return more derived types (covariance) or that accept parameters that have less derived types (contravariance) than that specified by the delegate type:

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

[↑ Using Variance in Delegates (C#)](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/using-variance-in-delegates).

## Links

[↑ C# Covariance](https://www.csharptutorial.net/csharp-tutorial/csharp-covariance/).

[↑ Variance in Generic Interfaces](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces).

[↑ Variance in Delegates](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/variance-in-delegates).

<sup>1</sup> A **method group** is the name for a set of methods (that might be just one) — i.e. in theory the `ToString` method may have multiple overloads (plus any extension methods): `ToString()`, `ToString(string format)`, etc — hence `ToString` by itself is a "method group".

It is purely a compiler term for "I know what the method name is, but I don't know the signature"; it has no existence at runtime. AFAIK, the only use of a method-group by itself (no brackets etc) is during delegate construction.

<https://metanit.com/sharp/tutorial/3.27.php>

[↑ How to use target typing and covariant returns in C# 9](https://www.infoworld.com/article/3613548/how-to-use-target-typing-and-covariant-returns-in-c-9.html).

[↑ Covariance and contravariance in generics](https://learn.microsoft.com/en-us/dotnet/standard/generics/covariance-and-contravariance).
