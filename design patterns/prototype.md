# Prototype

The **Prototype** is a creational software design pattern that lets you copy existing objects without making your code dependent on their classes.

## Participants

* **Prototype**
  * declares an interface for cloning itself.
* **ConcretePrototype**
  * implements an operation for cloning itself.
* **Client**
  * creates a new object by asking a prototype to clone itself.

## C# implementation

```csharp
using System;

class Program
{
    static void Main()
    {
        var original = new A { PropA = 1, PropB = new B { AnotherProp = 1 } };

        A shallow = original.CloneShallow();
        A deep = original.CloneDeep();

        Print(original, shallow, deep, "before");

        original.PropA = -1;
        original.PropB.AnotherProp = -1;

        Print(original, shallow, deep, "after");
    }

    private static void Print(A original, A shallow, A deep, string when)
    {
        Console.WriteLine($"Original {when}: {original.PropA}, {original.PropB.AnotherProp}");
        Console.WriteLine($"Shallow {when}: {shallow.PropA}, {shallow.PropB.AnotherProp}");
        Console.WriteLine($"Deep {when}: {deep.PropA}, {deep.PropB.AnotherProp}");    
    }
}

interface IClone
{
    A CloneShallow();
    A CloneDeep();
}

class A : IClone
{
    public int PropA { get; set; }

    public B PropB { get; set; }

    public A CloneShallow()
    {
        return (A) MemberwiseClone();
    }

    public A CloneDeep()
    {
        return new A
        {
            PropA = PropA,
            PropB = new B { AnotherProp = PropB.AnotherProp }
        };
    }
}

class B
{
    public int AnotherProp { get; set; }
}
```

Output:

```output
Original before: 1, 1
Shallow before: 1, 1
Deep before: 1, 1
Original after: -1, -1
Shallow after: 1, -1
Deep after: 1, 1
```

## See also

[Object.MemberwiseClone method](https://docs.microsoft.com/en-us/dotnet/api/system.object.memberwiseclone)

[ICloneable.Clone method](https://docs.microsoft.com/en-us/dotnet/api/system.icloneable.clone)
