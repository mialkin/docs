# Delegate

A delegate is a type that represents references to methods with a particular parameter list and return type.

The declaration of a delegate type is similar to a method signature. It has a return value and any number of parameters of any type:

```csharp
public delegate int Delegate(int x, int y);
```

Delegates are the basis for [events](event.md). A delegate can be instantiated by associating it either with a named or anonymous method.

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

## Remarks

The `Delegate` class is the base class for delegate types. However, only the system and compilers can derive explicitly from the `Delegate` class or from the `MulticastDelegate` class. It is also not permissible to derive a new type from a delegate type. The `Delegate` class is not considered a delegate type; it is a class used to derive delegate types.

## Links

[↑ Delegate Class](https://docs.microsoft.com/en-us/dotnet/api/system.delegate)
