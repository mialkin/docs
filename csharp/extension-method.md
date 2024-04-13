# Extension method

An **extension method** is a static methods defined in a static class, and called as if it was instance method of the type being extended.

Extension methods enable you to "add" methods to existing types without creating a new derived type, recompiling, or otherwise modifying the original type. For client code written in C#, there's no apparent difference between calling an extension method and the methods defined in a type.

The class that defines an extension methods is referred to as the "sponsor" class. To use extension methods, one must import the namespace defining the sponsor class.

## Table of contents

- [Extension method](#extension-method)
  - [Table of contents](#table-of-contents)
  - [Use cases](#use-cases)
  - [Extending predefined types](#extending-predefined-types)
  - [.NET Security](#net-security)
    - [Accessing private variables](#accessing-private-variables)
  - [Guidelines](#guidelines)
  - [Links](#links)

## Use cases

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

## Guidelines

❌ AVOID frivolously defining extension methods, especially on types you don't own.

If you do own source code of a type, consider using regular instance methods instead. If you don't own, and you want to add a method, be very careful. Liberal use of extension methods has the potential of cluttering APIs of types that were not designed to have these methods.

✔️ CONSIDER using extension methods in any of the following scenarios:

To provide helper functionality relevant to every implementation of an interface, if said functionality can be written in terms of the core interface. This is because concrete implementations cannot otherwise be assigned to interfaces. For example, the LINQ to objects operators are implemented as extension methods for all `IEnumerable<T>` types. Thus, any `IEnumerable<>` implementation is automatically LINQ-enabled.

When an instance method would introduce a dependency on some type, but such a dependency would break dependency management rules. For example, a dependency from `String` to `System.Uri` is probably not desirable, and so `String.ToUri()` instance method returning `System.Uri` would be the wrong design from a dependency management perspective. A static extension method `Uri.ToUri(this string str)` returning `System.Uri` would be a much better design.

❌ AVOID defining extension methods on `System.Object`.

VB users will not be able to call such methods on object references using the extension method syntax. VB does not support calling such methods because, in VB, declaring a reference as `Object` forces all method invocations on it to be late bound (actual member called is determined at run time), while bindings to extension methods are determined at compile-time (early bound).

Note that the guideline applies to other languages where the same binding behavior is present, or where extension methods are not supported.

❌ DO NOT put extension methods in the same namespace as the extended type unless it is for adding methods to interfaces or for dependency management.

❌ AVOID defining two or more extension methods with the same signature, even if they reside in different namespaces.

✔️ CONSIDER defining extension methods in the same namespace as the extended type if the type is an interface and if the extension methods are meant to be used in most or all cases.

❌ DO NOT define extension methods implementing a feature in namespaces normally associated with other features. Instead, define them in the namespace associated with the feature they belong to.

❌ AVOID generic naming of namespaces dedicated to extension methods (e.g., "Extensions"). Use a descriptive name (e.g., "Routing") instead.

* [↑ Extension Methods (Member Design Guidelines)](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/extension-methods)


## Links

[↑ Extension Methods (C# Programming Guide)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods).

[↑ How to implement and call a custom extension method](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/how-to-implement-and-call-a-custom-extension-method).
