# Nullable static analysis attributes

- [Nullable static analysis attributes](#nullable-static-analysis-attributes)
  - [AllowNull](#allownull)
  - [DisallowNull](#disallownull)
  - [MaybeNull](#maybenull)
  - [NotNull](#notnull)
  - [DoesNotReturn](#doesnotreturn)
  - [Links](#links)

You can apply a number of attributes that provide information to the compiler about the semantics of your APIs. That information helps the compiler perform static analysis and determine when a variable is not `null`.

All the examples assume C# 8.0 or newer, and the code is in a [↑ nullable context](https://docs.microsoft.com/en-us/dotnet/csharp/nullable-references#nullable-contexts).

Imagine your library has the following API to retrieve a resource string:

```csharp
bool TryGetMessage(string key, out string message)
```

This API has the following rules relating to the nullness of these arguments:

- Callers shouldn't pass `null` as the argument for `key`.
- Callers can pass a variable whose value is `null` as the argument for `message`.
- If the `TryGetMessage` method returns `true`, the value of `message` isn't `null`. If the return value is `false`, the value of `message` is `null`.

## AllowNull

The `AllowNull` attribute specifies that `null` is allowed as an input even if the corresponding type disallows it:

```csharp
using System.Diagnostics.CodeAnalysis;

class Example
{
    static void Main()
    {
        A a = new A();
        a.Act(null); // No compiler warning.
    }
}

class A
{
    public void Act([AllowNull]B b)
    {
    }
}

class B
{
}
```

## DisallowNull

Specifies that `null` is disallowed as an input even if the corresponding type allows it:

```csharp
using System.Diagnostics.CodeAnalysis;

class Example
{
    static void Main()
    {
        A a = new A();
        a.Act(null); // Compiler warning.
    }
}

class A
{
    public void Act([DisallowNull]B? b)
    {
    }
}

class B
{
}
```

## MaybeNull

Specifies that an output may be `null` even if the corresponding type disallows it:

```csharp
using System.Diagnostics.CodeAnalysis;

class Example
{
    static void Main()
    {
        A a = new A();
        a.Act();
    }
}

class A
{
    [return: MaybeNull]
    public B Act()
    {
        return null; // No compiler warning.
    }
}

class B
{
}
```

You can't specify that the return value of generic method is `T?` without constraints using constraints. The method returns `null` when the sought item isn't found. Since you can't declare a `T?` return type, you add the `MaybeNull` annotation to the method return:

```csharp
using System.Diagnostics.CodeAnalysis;

class Example
{
    static void Main()
    {
        A<B> a = new A<B>();

        B? b = a.Act();
    }
}

class A<T> where T : new()
{
    [return: MaybeNull]
    public T Act()
    {
        return default;
    }
}

class B
{
}
```

A nullable value type and a nullable reference type are fundamentally different. An `int?` is a synonym for `Nullable<int>`, whereas `string?` is `string` with an attribute added by the compiler. The result is that the compiler can't generate correct code for `T?` without knowing if `T` is a class or a struct.

This doesn't mean you can't use a nullable type (either value type or reference type) as the type argument for a closed generic type. Both `List<string?>` and `List<int?>` are valid instantiations of `List<T>`.

What it does mean is that you can't use `T?` in a generic class or method declaration without constraints. To overcome this limitation add either the `struct` or `class` constraint. With either of those constraints, the compiler knows how to generate code for both `T` and `T?`:

```csharp
using System.Diagnostics.CodeAnalysis;

class Example
{
    static void Main()
    {
        A<B> a = new A<B>();

        B? b = a.Act();
    }
}

class A<T> where T : class
{
    public T? Act()
    {
        return default;
    }
}

class B
{
}
```

## NotNull

Specifies that an output is not `null` even if the corresponding type allows it. Specifies that an input argument was not `null` when the call returns.

The `NotNull` attribute is most useful for `ref` and `out` arguments where `null` may be passed as an argument, but that argument is guaranteed to be not `null` when the method returns.

In the following example compiler generates an error "Program.cs(22, 5): [CS8777] Parameter 'b' must have a non-null value when exiting":

```csharp
using System;
using System.Diagnostics.CodeAnalysis;

class Example
{
    static void Main()
    {
        A a = new A();
        B b = new B();

        a.Act(ref b);
    }
}

class A
{
    public void Act([NotNull] ref B? b)
    {
        // b = new B();
        Console.WriteLine(".");
    }
}

class B
{
}
```

## DoesNotReturn

Specifies that a method that will never return under any circumstance.

Some methods, typically exception helpers or other utility methods, always exit by throwing an exception. In this case, you can add the `DoesNotReturn` attribute to the method declaration. The compiler helps you in three ways. First, the compiler issues a warning if there is a path where the method can exit without throwing an exception. Second, the compiler marks any code after a call to that method as unreachable, until an appropriate catch clause is encountered:

```csharp
[DoesNotReturn]
private void FailFast()
{
    throw new InvalidOperationException();
}

public void SetState(object containedField)
{
    if (!isInitialized)
    {
        FailFast();
    }

    // unreachable code:
    _field = containedField;
}
```

## Links

[Nullable reference types](types/nullable%20reference%20types.md)

[↑ System.Diagnostics.CodeAnalysis Namespace](https://docs.microsoft.com/en-us/dotnet/api/system.diagnostics.codeanalysis)

[↑ Nullable static analysis](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/attributes/nullable-analysis)
