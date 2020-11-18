# Constructor execution order

```csharp
using System;

class Program
{
    static void Main()
    {
        var b = new B();
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
```

Outputs:

```output
B static
A static
A
B
```

[↑ C# constructor execution order](https://stackoverflow.com/questions/1882692/c-sharp-constructor-execution-order)