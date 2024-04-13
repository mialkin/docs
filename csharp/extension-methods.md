# Extension method

An **extension method** is a static methods defined in a static class, and called as if it was instance method of the type being extended.

Extension methods enable you to "add" methods to existing types without creating a new derived type, recompiling, or otherwise modifying the original type. For client code written in C#, there's no apparent difference between calling an extension method and the methods defined in a type.

## Table of contents

- [Extension method](#extension-method)
  - [Table of contents](#table-of-contents)
  - [General guidelines](#general-guidelines)
  - [Extending predefined types](#extending-predefined-types)
  - [.NET Security](#net-security)
    - [Accessing private variables](#accessing-private-variables)
  - [Links](#links)

## General guidelines

While it's still considered preferable to add functionality by modifying an object's code or deriving a new type whenever it's reasonable and possible to do so, extension methods have become a crucial option for creating reusable functionality throughout the .NET ecosystem. For those occasions when the original source isn't under your control, when a derived object is inappropriate or impossible, or when the functionality shouldn't be exposed beyond its applicable scope, Extension methods are an excellent choice.

## Extending predefined types

Rather than creating new objects when reusable functionality needs to be created, we can often extend an existing type, such as a .NET or CLR type. For example you can add common functionality to the `System.String` class or extend the data processing capabilities of the `System.IO.File` and `System.IO.Stream` objects, and `System.Exception` objects for specific error handling functionality. These types of use-cases are limited only by your imagination and good sense.

Extending predefined types can be difficult with `struct` types because they're passed by value to methods. That means any changes to the `struct` are made to a copy of the `struct`. Those changes aren't visible once the extension method exits. Beginning with C# 7.2, you can add the `ref` modifier to the first argument of an extension method. Adding the `ref` modifier means the first argument is passed by reference. This enables you to write extension methods that change the state of the `struct` being extended.

## .NET Security

Extension methods present no specific security vulnerabilities. They can never be used to impersonate existing methods on a type, because all name collisions are resolved in favor of the instance or static method defined by the type itself.

You can use extension methods to extend a class or interface, but not to override them. An extension method with the same name and signature as an interface or class method will never be called. At compile time, extension methods always have lower priority than instance methods defined in the type itself.

### Accessing private variables

Extension methods cannot access any private data in the extended class:

```csharp
class User
{
    private readonly string _name;

    public User(string name)
    {
        _name = name;
    }

    // This works fine
    public string GetName => _name;
}

static class UserExtension
{
    public static void PrintName(this User user)
    {
        // Compilation error: "Cannot access private field '_name' here"
        Console.WriteLine(user._name);
    }
}
```

## Links

[↑ Extension Methods (C# Programming Guide)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods).

[↑ How to implement and call a custom extension method](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/how-to-implement-and-call-a-custom-extension-method).
