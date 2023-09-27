# Variance, invariance, covariance, contravariance

- [Variance, invariance, covariance, contravariance](#variance-invariance-covariance-contravariance)
  - [Variance](#variance)
  - [Invariance](#invariance)
  - [Covariance](#covariance)
    - [Arrays](#arrays)
    - [Interfaces](#interfaces)
  - [Contravariance](#contravariance)
    - [Interfaces](#interfaces-1)
  - [Delegates](#delegates)
    - [Covariance and contravariance](#covariance-and-contravariance)
  - [Excerpt from Jeffrey Richter's "CLR via C#" book](#excerpt-from-jeffrey-richters-clr-via-c-book)
  - [Links](#links)

## Variance

Covariance and contravariance are collectively referred to as variance.

## Invariance

<https://learn.microsoft.com/en-us/dotnet/standard/generics/covariance-and-contravariance>

## Covariance

A **covariance** is a feature that allows to use a more derived type in the context that requires a less derived type.

### Arrays

Object array is not type safe and should not be used; it is used here just for demonstration of covariance:

```csharp
Base[] array =
{
    new() { Id = 1 },
    new Derived { Id = 2, Name = "Second" },
};

var first = array[0];
var second = (Derived) array[1];

Console.WriteLine(first.Id);
Console.WriteLine("{0} {1}", second.Id, second.Name);

class Base
{
    public int Id { get; init; }
}

class Derived : Base
{
    public string? Name { get; init; }
}
```

Output:

```console
1
2 Second
```

### Interfaces

A covariant interface allows its methods to return more derived types than those specified in the interface:

```csharp
IEnumerable<object> list = new List<string>();
```

Another example which would not work without `out` keyword:

```csharp
IA<Base> a = new A<Derived>();

interface IA<out T>
{
    T? Act();
}

class A<T> : IA<T>
{
    public T? Act() => default;
}

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
// IEnumerable<object> objects = integers;
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

## Excerpt from Jeffrey Richter's "CLR via C#" book

- **Invariant** Meaning that the generic type parameter cannot be changed. I have shown only
invariant generic type parameters so far in this chapter.
- **Contra-variant** Meaning that the generic type parameter can change from a class to a
class derived from it. In C#, you indicate contra-variant generic type parameters with the in
keyword. Contra-variant generic type parameters can appear only in input positions such as a
method’s argument.
- **Covariant** Meaning that the generic type argument can change from a class to one of its
base classes. In C#, you indicate covariant generic type parameters with the out keyword. Covariant generic type parameters can appear only in output positions such as a method’s return
type.

## Links

[↑ Variance in Generic Interfaces](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces)

[↑ Variance in Delegates](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/variance-in-delegates)

<sup>1</sup> A **method group** is the name for a set of methods (that might be just one) — i.e. in theory the `ToString` method may have multiple overloads (plus any extension methods): `ToString()`,` ToString(string format)`, etc — hence `ToString` by itself is a "method group".

It is purely a compiler term for "I know what the method name is, but I don't know the signature"; it has no existence at runtime. AFAIK, the only use of a method-group by itself (no brackets etc) is during delegate construction.

<https://metanit.com/sharp/tutorial/3.27.php>
