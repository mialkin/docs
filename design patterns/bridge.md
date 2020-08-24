# Bridge

The **bridge** is a structural software desing pattern that decouples an abstraction from its implementation so that the two can vary
independently.

## Participants

* **Abstraction**
  * defines the abstraction's interface.
  * maintains a reference to an object of type Implementor.
* **RefinedAbstraction**
  * Extends the interface defined by Abstraction.
* **Implementor**
  * defines the interface for implementation classes. This interface
doesn't have to correspond exactly to Abstraction's interface; in
fact the two interfaces can be quite different. Typically the
Implementor interface provides only primitive operations, and
Abstraction defines higher-level operations based on these
primitives.
* **ConcreteImplementor**
  * implements the Implementor interface and defines its concrete
implementation.

## C# implementation

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

## Related Patterns

An Abstract Factory can create and configure a particular Bridge.
The Adapter pattern is geared toward making unrelated classes work together.
It is usually applied to systems after they're designed. Bridge, on the other
hand, is used up-front in a design to let abstractions and implementations vary independently.
