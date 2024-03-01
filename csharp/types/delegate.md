# `delegate`

The `delegate` is a keyword used to define a _delegate type_.

## Table of contents

- [`delegate`](#delegate)
  - [Table of contents](#table-of-contents)
  - [Delegate type](#delegate-type)
  - [Delegate](#delegate-1)
  - [Variance in delegates](#variance-in-delegates)
  - [`MulticastDelegate`](#multicastdelegate)
    - [Returning value](#returning-value)
  - [Remarks](#remarks)
  - [Connection to `event` keyword](#connection-to-event-keyword)
  - [Connection to lambda expressions](#connection-to-lambda-expressions)

## Delegate type

A **delegate type** is a reference type that can be used to encapsulate a named or an anonymous method. Delegates are similar to function pointers in C++; however, delegates are type-safe and secure.

You define a delegate type using syntax that is similar to defining a method signature, you just add the `delegate` keyword to the definition:

`MyDelegateType.cs`:

```csharp
public delegate int MyDelegateType(int x, int y);
```

Because `MyDelegateType` is a _type_ defined with `delegate` keyword you can put it in a separate `MyDelegateType.cs` file, as you would do, for example, with a user-defined type using `class`, `interface`, `enum` or any other keyword.

## Delegate

The word **delegate**, depending on context, can mean one of two things:

1. An instance or a variable of a delegate type
2. Delegate type

Usually by saying "delegate" they mean the second thing — delegate type.

## Variance in delegates

Methods do not have to match the delegate type exactly. See [variance in delegates](../invariance-covariance-contravariance-variance.md).

## `MulticastDelegate`

[↑ Every](https://learn.microsoft.com/en-us/dotnet/csharp/delegate-class#delegate-and-multicastdelegate-classes) delegate you work with is derived from `MulticastDelegate`.

[↑ Is there a Delegate which isn't a MulticastDelegate in C#?](https://stackoverflow.com/questions/4711118/is-there-a-delegate-which-isnt-a-multicastdelegate-in-c).

`MulticastDelegate` represents a multicast delegate; that is, a delegate that can have more than one element in its invocation list:

```csharp
public abstract class MulticastDelegate : Delegate
```

A useful property of delegate objects is that multiple objects can be assigned to one delegate instance by using the `+` operator. The multicast delegate contains a list of the assigned delegates. When the multicast delegate is called, it invokes the delegates in the list, in order. Only delegates of the same type can be combined.

The `-` operator can be used to remove a component delegate from a multicast delegate.

`PrintingDelegate.cs`:

```csharp
delegate void PrintingDelegate(string message);
```

`Program.cs`:

```csharp
void LocalPrintFunction(string message)
{
    Console.WriteLine(message + "local function");
}

PrintingDelegate printingDelegate = message => Console.WriteLine(message + "lambda function");
printingDelegate += LocalPrintFunction;

printingDelegate("Printing from ");

```

Output:

```output
Printing from lambda function
Printing from local function
```

### Returning value

The behaviour of a multicast delegate when invoked is as follows: the methods that the delegate object points to are invoked in sequence in the order that they were added to the delegate, and the return value of the delegate is the return value of the last method invoked – all the others are discarded.

`MyDelegateType.cs`:

```csharp
// Define delegate type
public delegate string MyDelegateType(int x, int y);
```

`Program.cs`:

```csharp
// Create delegate
MyDelegateType instance = (x, y) =>
{
    Console.WriteLine("Adding tilda");
    return x.ToString() + '~' + y;
};

instance += (x, y) =>
{
    Console.WriteLine("Adding dash");
    return x.ToString() + '-' + y;
};

// Call delegate
var result = instance(10, 20);
Console.WriteLine(result);
```

This outputs, as you can see below, only `10-20` since `10~20` is discarded:

```console
Adding tilda
Adding dash
10-20
```

## Remarks

`MulticastDelegate` is a special class. Compilers and other tools can derive from this class, but you cannot derive from it explicitly. The same is true of the `Delegate` class.

The `Delegate` class is the base class for delegate types. The `Delegate` class is not considered a delegate type; it is a class used to derive delegate types.

## Connection to `event` keyword

Delegates are the basis for [events](/csharp/keywords/event.md).

## Connection to lambda expressions

[Lambda expressions](/csharp/operators/lambda-expressions.md) in certain contexts are compiled to delegate types.
