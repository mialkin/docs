# Mediator

The **Mediator** is a behavioral software design pattern that defines an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently.

## Participants

* **Mediator**
  * defines an interface for communicating with Colleague objects.
* **ConcreteMediator**
  * implements cooperative behavior by coordinating Colleague objects.
  * knows and maintains its colleagues.
* **Colleague classes**
  * each Colleague class knows its Mediator object.
  * each colleague communicates with its mediator whenever it would have otherwise communicated with another colleague.

## C# implementation

```csharp
using System;

class Program
{
    public static void Main()
    {
        var a = new A();
        var b = new B();
        var c = new C();

        var mediator = new Mediator(a, b, c);

        a.Act();
    }
}

interface IMediator
{
    void SendMsgFromBToA(string msg);
    void SendMsgFromAToB(string msg);
    void SendMsgToAll(string msg);
}

class Mediator : IMediator
{
    private readonly A _a;
    private readonly B _b;
    private readonly C _c;

    public Mediator(A a, B b, C c)
    {
        _a = a;
        _b = b;
        _c = c;

        _a.SetMediator(this);
        _b.SetMediator(this);
        _c.SetMediator(this);
    }

    public void SendMsgFromBToA(string msg)
    {
        _a.GetMsgFromB(msg);
    }

    public void SendMsgFromAToB(string msg)
    {
        _b.GetMsgFromA(msg);
    }

    public void SendMsgToAll(string msg)
    {
        _a.GetMsgFromSomeone(msg);
        _b.GetMsgFromSomeone(msg);
        _c.GetMsgFromSomeone(msg);
    }
}

class A
{
    private IMediator _mediator;

    public void SetMediator(IMediator mediator)
    {
        _mediator = mediator;
    }

    public void Act()
    {
        Console.WriteLine("Doing work in A");

        _mediator.SendMsgFromAToB("Hi B! It is A");
    }

    public void GetMsgFromB(string msg)
    {
        Console.WriteLine($"A got message from B: {msg}");
    }

    public void GetMsgFromSomeone(string msg)
    {
        Console.WriteLine($"A got message from someone: {msg}");
    }
}

class B
{
    private IMediator _mediator;

    public void SetMediator(IMediator mediator)
    {
        _mediator = mediator;
    }

    public void GetMsgFromA(string msg)
    {
        Console.WriteLine($"B got message from A: {msg}");
        _mediator.SendMsgFromBToA("Hi A! It is B");
        _mediator.SendMsgToAll("Hi everyone! It is B");
    }

    public void GetMsgFromSomeone(string msg)
    {
        Console.WriteLine($"B got message from someone: {msg}");
    }
}

class C
{
    private IMediator _mediator;

    public void SetMediator(IMediator mediator)
    {
        _mediator = mediator;
    }

    public void GetMsgFromSomeone(string msg)
    {
        Console.WriteLine($"C got message from someone: {msg}");
    }
}
```

Output:

```output
Doing work in A
B got message from A: Hi B! It is A
A got message from B: Hi A! It is B
A got message from someone: Hi everyone! It is B
B got message from someone: Hi everyone! It is B
C got message from someone: Hi everyone! It is B
```
