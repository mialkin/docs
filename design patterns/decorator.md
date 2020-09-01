# Decorator

The **Decorator** is a structural software desing pattern that attaches additional responsibilities to an object dynamically. Decorators provides a flexible alternative to subclassing for extending functionality.

## Participants

* **Component**
  * defines the interface for objects that can have responsibilities added to them dynamically.
* **ConcreteComponent**
  * defines an object to which additional responsibilities can be attached.
* **Decorator**
  * maintains a reference to a Component object and defines an interface that conforms to Component's interface.
* **ConcreteDecorator**
  * adds responsibilities to the component.

## C# implementation

```csharp
using System;

class Program
{
    static void Main()
    {
        IComponent component = new Component();
        Console.WriteLine("== Pure component acts ==");
        component.Act();

        Console.WriteLine("== The same but decorated component acts ==");
        IDecorator decorator = new Decorator(component);
        Client(decorator);
    }

    static void Client(IComponent component)
    {
        component.Act();
    }
}

interface IComponent
{
    void Act();
}

class Component : IComponent
{
    public void Act()
    {
        Console.WriteLine("Component acts");
    }
}

interface IDecorator : IComponent
{
}

class Decorator : IDecorator
{
    private readonly IComponent _component;

    public Decorator(IComponent component)
    {
        _component = component;
    }

    public void Act()
    {
        Console.WriteLine("Decorator acts");
        _component.Act();
        Console.WriteLine("Decorator acts again");
    }
}
```

Output:

```csharp
== Pure component acts ==
Component acts
== The same but decorated component acts ==
Decorator acts
Component acts
Decorator acts again
```
