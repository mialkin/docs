# Bridge

```csharp
using System;

class Program
{
    static void Main()
    {
        IAbstraction abstraction = new Abstraction(new Implementation());
        abstraction.A();
    }
}

interface IAbstraction
{
    void A();
    void B();
}

interface IImplementation
{
    void C();
    void D();
    void E();
}

class Abstraction : IAbstraction
{
    private readonly IImplementation _implementation;

    public Abstraction(IImplementation implementation)
    {
        _implementation = implementation;
    }

    public void A()
    {
        _implementation.C();
        Console.WriteLine("A");
        _implementation.E();
    }

    public void B()
    {
        Console.WriteLine("B");
        _implementation.D();
    }
}

class Implementation : IImplementation
{
    public void C()
    {
        Console.WriteLine("C");
    }

    public void D()
    {
        Console.WriteLine("D");
    }

    public void E()
    {
        Console.WriteLine("E");
    }
}
```

Output:

```output
C
A
E
```
