# Extension methods

Extension methods enable you to "add" methods to existing types without creating a new derived type, recompiling, or otherwise modifying the original type. Extension methods are static methods, but they're called as if they were instance methods on the extended type. For client code written in C#, there's no apparent difference between calling an extension method and the methods defined in a type.

## Common Usage Patterns

### Collection Functionality

In the past, it was common to create "Collection Classes" that implemented the `System.Collections.Generic.IEnumerable<T>` interface for a given type and contained functionality that acted on collections of that type. While there's nothing wrong with creating this type of collection object, the same functionality can be achieved by using an extension on the `System.Collections.Generic.IEnumerable<T>`. Extensions have the advantage of allowing the functionality to be called from any collection such as an `System.Array` or `System.Collections.Generic.List<T>` that implements `System.Collections.Generic.IEnumerable<T>` on that type.

Example:

```csharp
using System;

class Program
{
    static void Main()
    {
        IA a = new A();
        a.Extend();

        IA b = new B();
        b.Extend();
    }
}

interface IA
{
    public int X { get; set; }
}

class A : IA
{
    public int X { get; set; } = 5;
}

class B : IA
{
    public int X { get; set; } = 20;
}

static class DoesNotMatterNameExtension
{
    public static void Extend(this IA ia)
    {
        Console.WriteLine($"Value of X is {ia.X}");
    }
}
```

Output:

```text
Value of X is 5
Value of X is 20
```

### Layer-Specific Functionality

When using an Onion Architecture or other layered application design, it's common to have a set of Domain Entities or Data Transfer Objects that can be used to communicate across application boundaries. These objects generally contain no functionality, or only minimal functionality that applies to all layers of the application. Extension methods can be used to add functionality that is specific to each application layer without loading the object down with methods not needed or wanted in other layers:

```csharp
public class DomainEntity
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

static class DomainEntityExtensions
{
    static string FullName(this DomainEntity value) => $"{value.FirstName} {value.LastName}";
}
```

## Extending Predefined Types

Rather than creating new objects when reusable functionality needs to be created, we can often extend an existing type, such as a .NET or CLR type. For example you can add common functionality to the `System.String` class or extend the data processing capabilities of the `System.IO.File` and `System.IO.Stream` objects, and `System.Exception` objects for specific error handling functionality. These types of use-cases are limited only by your imagination and good sense.

Extending predefined types can be difficult with `struct` types because they're passed by value to methods. That means any changes to the `struct` are made to a copy of the `struct`. Those changes aren't visible once the extension method exits. Beginning with C# 7.2, you can add the `ref` modifier to the first argument of an extension method. Adding the `ref` modifier means the first argument is passed by reference. This enables you to write extension methods that change the state of the `struct` being extended.

## General Guidelines

While it's still considered preferable to add functionality by modifying an object's code or deriving a new type whenever it's reasonable and possible to do so, extension methods have become a crucial option for creating reusable functionality throughout the .NET ecosystem. For those occasions when the original source isn't under your control, when a derived object is inappropriate or impossible, or when the functionality shouldn't be exposed beyond its applicable scope, Extension methods are an excellent choice.

## Accessing private variables

It is not possible to access object's private variables inside extention method:

```csharp
using System;

class Program
{
}

class A
{
    private static int X { get; set; }


    public static void Ex()
    {
        Console.WriteLine(X); // This works fine as expected.
    }
}

static class WhateverNameExtension
{
    public static void Extend(this A a)
    {
        Console.WriteLine(a.X); // This will not compile.
    }
}
```

## .NET Security

Extension methods present no specific security vulnerabilities. They can never be used to impersonate existing methods on a type, because all name collisions are resolved in favor of the instance or static method defined by the type itself. Extension methods cannot access any private data in the extended class.

You can use extension methods to extend a class or interface, but not to override them. An extension method with the same name and signature as an interface or class method will never be called. At compile time, extension methods always have lower priority than instance methods defined in the type itself.

## Links

* [↑ Extension Methods (C# Programming Guide)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)
* [↑ How to implement and call a custom extension method](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/how-to-implement-and-call-a-custom-extension-method)
