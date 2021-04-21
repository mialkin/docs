# Constructor execution order

A static constructor is used to initialize any static data, or to perform a particular action that needs to be performed only once. It is called automatically before the first instance is created or any static members are referenced.

Check out [Remarks ↑](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/static-constructors#remarks) for more details on static constructors.

## Static constructor

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

Outputs:

```output
B static
A static
A
B
```

## Non-static constructor

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

Outputs:

```output
A
B
```

[↑ C# constructor execution order](https://stackoverflow.com/questions/1882692/c-sharp-constructor-execution-order)