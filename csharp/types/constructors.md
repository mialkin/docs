# Constructors

- [Constructors](#constructors)
  - [Private constructors](#private-constructors)
  - [Static constructors](#static-constructors)
  - [Constructor execution order](#constructor-execution-order)

## Private constructors

https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/private-constructors

## Static constructors

https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/static-constructors

## Constructor execution order

Static constructor:

```csharp
using System;

var b = new B();

class B : A
{
    static B()
    {
        Console.WriteLine("B static");
    }

    public B()
    {
        Console.WriteLine("B");
    }
}

class A
{
    static A()
    {
        Console.WriteLine("A static");
    }

    public A()
    {
        Console.WriteLine("A");
    }
}
```

Output:

```output
B static
A static
A
B
```

Non-static constructor:

```csharp
using System;

var b = new B();

class B : A
{
    public B()
    {
        Console.WriteLine("B");
    }
}

class A
{
    public A()
    {
        Console.WriteLine("A");
    }
}
```

Output:

```output
A
B
```

[↑ C# constructor execution order](https://stackoverflow.com/questions/1882692/c-sharp-constructor-execution-order)
