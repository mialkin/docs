# Prototype

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
        Console.WriteLine($"Deep {when}: {deep.PropA}, {deep.PropB.AnotherProp}");    }
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

## See also

[Object.MemberwiseClone method](https://docs.microsoft.com/en-us/dotnet/api/system.object.memberwiseclone)
