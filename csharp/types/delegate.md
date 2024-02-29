# `delegate`

The `delegate` is a keyword used to define a *delegate type*.

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

## Delegate type

You define a **delegate type** using syntax that is similar to defining a method signature, you just add the `delegate` keyword to the definition:

`MyDelegateType.cs`:

```csharp
public delegate int MyDelegateType(int x, int y);
```

Because `MyDelegateType` is a *type* defined with `delegate` keyword you can put it in a separate `MyDelegateType.cs` file, as you would do, for example, with a user-defined type using `class`, `interface`, `enum` or any other keyword.

## Delegate

A **delegate** is an instance of a delegate type.

## Variance in delegates

Methods do not have to match the delegate type exactly. See [variance in delegates](../invariance-covariance-contravariance-variance.md).

## `MulticastDelegate`

[↑ Every](https://learn.microsoft.com/en-us/dotnet/csharp/delegate-class#delegate-and-multicastdelegate-classes) delegate you work with is derived from `MulticastDelegate`.

`MulticastDelegate` represents a multicast delegate; that is, a delegate that can have more than one element in its invocation list:

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

### Returning value

The behaviour of a multicast delegate when invoked is as follows: the methods that the delegate object points to are invoked in sequence in the order that they were added to the delegate, and the return value of the delegate is the return value of the last method invoked – all the others are discarded.

## Remarks

`MulticastDelegate` is a special class. Compilers and other tools can derive from this class, but you cannot derive from it explicitly. The same is true of the `Delegate` class.

The `Delegate` class is the base class for delegate types. The `Delegate` class is not considered a delegate type; it is a class used to derive delegate types.

## Connection to `event` keyword

Delegates are the basis for [events](/csharp/keywords/event.md).
