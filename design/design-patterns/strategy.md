# Strategy

The **Strategy** is a behavioral software design pattern that enables selecting an algorithm at runtime. Instead of implementing a single algorithm directly, code receives run-time instructions as to which in a family of algorithms to use.

## Participants

* **Strategy**
  * declares an interface common to all supported algorithms. Context uses this interface to call the algorithm defined by a ConcreteStrategy.
* **ConcreteStrategy**
  * implements the algorithm using the Strategy interface.
* **Context**
  * is configured with a ConcreteStrategy object.
  * maintains a reference to a Strategy object.
  * may define an interface that lets Strategy access its data.

## C# implementation

```csharp
using System;

class Program
{
    public static void Main()
    {
        var context = new Context(new SlowStrategy());
        context.Act();

        context.SetStrategy(new FastStrategy());
        context.Act();
    }
}

interface IStrategy
{
    void Act();
}

class SlowStrategy : IStrategy
{
    public void Act()
    {
        Console.WriteLine("Slow");
    }
}

class FastStrategy : IStrategy
{
    public void Act()
    {
        Console.WriteLine("Fast");
    }
}

class Context
{
    private IStrategy _strategy;

    public Context(IStrategy strategy)
    {
        _strategy = strategy;
    }

    public void Act()
    {
        _strategy.Act();
    }

    public void SetStrategy(IStrategy strategy)
    {
        _strategy = strategy;
    }
}
```

Output:

```output
Slow
Fast
```

## See also

[↑ Composition over inheritance](../composition-over-inheritance.md).
