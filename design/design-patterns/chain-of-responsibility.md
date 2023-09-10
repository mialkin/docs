# Chain of responsibility

The **Chain of responsibility** is a behavioral software design pattern that avoids coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handles it.

## Participants

* **Handler**
  * defines an interface for handling requests.
  * (optional) implements the successor link.
* **ConcreteHandler**
  * handles requests it is responsible for.
  * can access its successor.
  * if the ConcreteHandler can handle the request, it does so; otherwise it forwards the request to its successor.
* **Client**
  * initiates the request to a ConcreteHandler object on the chain.

## C# implementation

```csharp
using System;
using System.Collections.Generic;

class Program
{
    public static void Main()
    {
        IHandler h1 = new Handler1();
        IHandler h2 = new Handler2();
        IHandler h3 = new Handler3();

        h1.SetSuccessor(h2);
        h2.SetSuccessor(h3);

        var list = new List<int> { 2, 12, 5, 0, 15, 8, 30 };

        foreach (int item in list)
        {
            h1.Handle(item);
        }
    }
}

interface IHandler
{
    void SetSuccessor(IHandler successor);
    void Handle(int num);
}

class Handler1 : IHandler
{
    private IHandler _successor;

    public void SetSuccessor(IHandler successor)
    {
        _successor = successor;
    }

    public void Handle(int num)
    {
        if (num > 4 && num < 10)
        {
            Console.WriteLine($"Handler1: {num}");
            return;
        }

        _successor?.Handle(num);
    }
}

class Handler2 : IHandler
{
    private IHandler _successor;

    public void SetSuccessor(IHandler successor)
    {
        _successor = successor;
    }

    public void Handle(int num)
    {
        if (num > 10 && num < 20)
        {
            Console.WriteLine($"Handler2: {num}");
            return;
        }

        _successor?.Handle(num);
    }
}

class Handler3 : IHandler
{
    private IHandler _successor;

    public void SetSuccessor(IHandler successor)
    {
        _successor = successor;
    }

    public void Handle(int num)
    {
        Console.WriteLine($"Handler3 (default): {num}");
    }
}
```

Output:

```output
Handler3 (default): 2
Handler2: 12
Handler1: 5
Handler3 (default): 0
Handler2: 15
Handler1: 8
Handler3 (default): 30
```

## Related patterns

Chain of responsibility is often applied in conjunction with [Composite](composite.md). There, a component's parent can act as its successor.
