# Invariance, covariance, contravariance, variance

## Table of contents

- [Invariance, covariance, contravariance, variance](#invariance-covariance-contravariance-variance)
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

At the time a covariant interface is defined, we do not know what type of object will be returned in its method.

The `out` keyword in the interface definition indicates that this interface will be covariant

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

#### Example 1

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

[↑ Using Variance in Delegates (C#)](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/using-variance-in-delegates).

## Variance

Covariance and contravariance are collectively referred to as **variance**.

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
