# Delegate

A **delegate** is a *type* that represents references to methods with a particular parameter list and return type. When you instantiate a delegate, you can associate its instance with any method with a compatible signature and return type. You can invoke (or call) the method through the delegate instance.

Delegates are used to pass methods as arguments to other methods. Event handlers are nothing more than methods that are invoked through delegates. You create a custom method, and a class such as a windows control can call your method when a certain event occurs.

Delegates are similar to C++ function pointers, but delegates are fully object-oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.

Delegates can be chained together; for example, multiple methods can be called on a single event.

Methods do not have to match the delegate type exactly. For more information, see [variance in delegates](../covariance%20and%20contravariance.md).

The declaration of a delegate type is similar to a method signature. It has a return value and any number of parameters of any type:

```csharp
public delegate int Delegate(int x, int y);
```

Delegates are the basis for [events](../keywords/event.md). A delegate can be instantiated by associating it either with a named or anonymous method.

```csharp
class Program
{
    private delegate int Dele(string str);

    public static void Main()
    {
        Dele d = str => str.Length;
        Dele d2 = delegate(string str) { return str.Length; };
        Dele d3 = D3;
    }

    private static int D3(string str)
    {
        return str.Length;
    }
}
```

## MulticastDelegate

Represents a multicast delegate; that is, a delegate that can have more than one element in its invocation list.

```csharp
public abstract class MulticastDelegate : Delegate
```

A useful property of delegate objects is that multiple objects can be assigned to one delegate instance by using the `+` operator. The multicast delegate contains a list of the assigned delegates. When the multicast delegate is called, it invokes the delegates in the list, in order. Only delegates of the same type can be combined.

The `-` operator can be used to remove a component delegate from a multicast delegate:

```csharp
using System;

class Program
{
    private delegate int Whatever(int value);

    static void Main(string[] args)
    {
        Whatever d = x =>
        {
            Console.WriteLine($"x + 1 called with x = {x}");
            return x + 1;
        };

        d += x =>
        {
            Console.WriteLine($"x * 2 called with x = {x}");
            return x * 2;
        };

        int result = d.Invoke(3);
        int result2 = d.Invoke(5);

        Console.WriteLine(result);
        Console.WriteLine(result2);

        int result3 = d.Invoke(3);
        int result4 = d.Invoke(5);

        Console.WriteLine(result3);
        Console.WriteLine(result4);
    }
}
```

Output:

```output
x + 1 called with x = 3
x * 2 called with x = 3
x + 1 called with x = 5
x * 2 called with x = 5
6
10
x + 1 called with x = 3
x * 2 called with x = 3
x + 1 called with x = 5
x * 2 called with x = 5
6
10
```

## Remarks

The `Delegate` class is the base class for delegate types. However, only the system and compilers can derive explicitly from the `Delegate` class or from the `MulticastDelegate` class. It is also not permissible to derive a new type from a delegate type. The `Delegate` class is not considered a delegate type; it is a class used to derive delegate types.

## Links

[↑ C# in Depth — Delegates and Events](https://csharpindepth.com/Articles/Events)

[↑ Delegate Class](https://docs.microsoft.com/en-us/dotnet/api/system.delegate)
