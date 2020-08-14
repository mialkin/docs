# Strategy

The **strategy pattern** is a behavioral software design pattern that enables selecting an algorithm at runtime. Instead of implementing a single algorithm directly, code receives run-time instructions as to which in a family of algorithms to use.

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

public class Program
{
    public static void Main()
    {
        var context = new Context(new StrategyOne());
        context.Act();
        
        context.SetStrategy(new StrategyTwo());
        context.Act();
    }
}

public interface IStrategy
{
    void Act();
}

public class StrategyOne : IStrategy
{
    public void Act()
    {
        Console.WriteLine("One");
    }
}

public class StrategyTwo : IStrategy
{
    public void Act()
    {
        Console.WriteLine("Two");
    }
}

public class Context
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

```console
One
Two
```

## See also

[Composition over inheritance](/software%20design/composition%20over%20inheritance.md)
