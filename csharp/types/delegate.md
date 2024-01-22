# `delegate`

The `delegate` is a reference type that represents references to methods with a particular parameter list and return type.

A **delegate** is a code, a class, that the compiler generates when you use the `delegate` keyword.

You define a **delegate type** using syntax that is similar to defining a method signature; you just add the `delegate` keyword to the definition.

[↑ Every](https://learn.microsoft.com/en-us/dotnet/csharp/delegate-class#delegate-and-multicastdelegate-classes) delegate you work with is derived from `MulticastDelegate`.

When you instantiate a delegate, you can associate its instance with any method with a compatible signature and return type. You can invoke, or call, the method through the delegate instance:

```csharp
PerformCalculation function = (x, y) => x + y;

var result = function(1, 2);
Console.WriteLine(result);

delegate int PerformCalculation(int x, int y);
```

Delegates are used to pass methods as arguments to other methods. Event handlers are nothing more than methods that are invoked through delegates. You create a custom method, and a class, such as a windows control, can call your method when a certain event occurs.

Delegates are similar to C++ function pointers, but delegates are fully object-oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.

Delegates can be chained together; for example, multiple methods can be called on a single event.

Methods do not have to match the delegate type exactly. For more information, see [variance in delegates](../invariance-covariance-contravariance-variance.md).

The declaration of a delegate type is similar to a method signature. It has a return value and any number of parameters of any type:

```csharp
public delegate int Delegate(int x, int y);
```

Delegates are the basis for [events](../keywords/event.md). A delegate can be instantiated by associating it either with a named or anonymous method.

```csharp
GetStringLength delegate1 = x => x.Length;
GetStringLength delegate2 = GetLength;

Console.WriteLine(delegate1("Summer"));
Console.WriteLine(delegate2("Fall"));

int GetLength(string @string)
{
    return @string.Length;
}

delegate int GetStringLength(string str);
```

## Table of contents

- [`delegate`](#delegate)
  - [Table of contents](#table-of-contents)
  - [`MulticastDelegate`](#multicastdelegate)
  - [Remarks](#remarks)
  - [Links](#links)

## `MulticastDelegate`

Represents a multicast delegate; that is, a delegate that can have more than one element in its invocation list:

```csharp
public abstract class MulticastDelegate : Delegate
```

A useful property of delegate objects is that multiple objects can be assigned to one delegate instance by using the `+` operator. The multicast delegate contains a list of the assigned delegates. When the multicast delegate is called, it invokes the delegates in the list, in order. Only delegates of the same type can be combined.

The `-` operator can be used to remove a component delegate from a multicast delegate.

```csharp
Print print = message => Console.WriteLine(message + "lambda function");
print += LocalPrintFunction;

print("Printing from ");

void LocalPrintFunction(string message)
{
    Console.WriteLine(message + "local function");
}

delegate void Print(string str);
```

Output:

```output
Printing from lambda function
Printing from local function
```

[↑ Is there a Delegate which isn't a MulticastDelegate in C#?](https://stackoverflow.com/questions/4711118/is-there-a-delegate-which-isnt-a-multicastdelegate-in-c)

## Remarks

`MulticastDelegate` is a special class. Compilers and other tools can derive from this class, but you cannot derive from it explicitly. The same is true of the `Delegate` class.

The `Delegate` class is the base class for delegate types. The `Delegate` class is not considered a delegate type; it is a class used to derive delegate types.

## Links

[↑ Delegates and Events](https://csharpindepth.com/Articles/Events).

[↑ Delegate Class](https://docs.microsoft.com/en-us/dotnet/api/system.delegate).
