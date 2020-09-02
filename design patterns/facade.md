# Facade

The **Facade** is a structural software desing pattern that provides a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.

## Participants

* **Facade**
  * knows which subsystem classes are responsible for a request.
  * delegates client requests to appropriate subsystem objects.
* **subsystem classes**
  * implement subsystem functionality.
  * handle work assigned by the Facade object.
  * have no knowledge of the facade; that is, they keep no references to it.

## C# implementation

```csharp
using System;

class Program
{
    static void Main()
    {
        IFacade facade = new Facade();
        Client(facade);
    }

    static void Client(IFacade facade)
    {
        facade.Act();
    }
}

interface IFacade
{
    void Act();
}

class Facade : IFacade
{
    private readonly IA _a;
    private readonly IB _b;
    private readonly IC _c;

    public Facade()
    {
        _a = new A();
        _b = new B();
        _c = new C();
    }

    public void Act()
    {
        Console.WriteLine("Doing the job inside facade");
        _a.Query();
        _c.Process();
        _c.Run(_b.Initialize());
    }
}

interface IA
{
    void Query();
}

class A : IA
{
    public void Query()
    {
        Console.WriteLine("Query");
    }
}

interface IB
{
    int Initialize();
}

class B : IB
{
    public int Initialize()
    {
        Console.WriteLine("Initialize");
        return 7;
    }
}

interface IC
{
    void Process();
    void Run(int num);
}

class C : IC
{
    public void Process()
    {
        Console.WriteLine("Process");
    }

    public void Run(int num)
    {
        Console.WriteLine($"{num}Run");
    }
}
```
