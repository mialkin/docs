# Adapter

The **adapter** is a structural software design pattern that converts the interface of a class into another interface clients expect. An adapter lets classes work together that could not otherwise because of incompatible interfaces.

## Participants

* **Target**
  * defines the domain-specific interface that Client uses.
* **Client**
  * collaborates with objects conforming to the Target interface.
* **Adaptee**
  * defines an existing interface that needs adapting.
* **Adapter**
  * adapts the interface of Adaptee to the Target interface.

## C# implementation

```csharp
using System;

class Program
{
    static void Main()
    {
        ITarget adapter = new Adapter(new A());
        Client(adapter);
    }

    static void Client(ITarget target)
    {
        target.MethodB();
    }
}

interface IAdaptee
{
    void MethodA();
}

interface ITarget
{
    void MethodB();
}

class A : IAdaptee
{
    public void MethodA()
    {
        Console.WriteLine("A");
    }
}

class Adapter : ITarget
{
    private IAdaptee _adaptee;
    
    public Adapter(IAdaptee adaptee)
    {
        _adaptee = adaptee;
    }

    public void MethodB()
    {
        _adaptee.MethodA();
    }
}
```

Output:

```output
A
```
