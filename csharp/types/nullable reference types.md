# Nullable reference types

All code you wrote before C# 8 introduced nullable reference types was _null oblivious_. That means any reference type variable may be `null`, but null checks aren't required. Once your code is _nullable aware_, those rules change. Reference types should never be the `null` value, and nullable reference types must be checked against `null` before being dereferenced.

C# 8.0 introduces **nullable reference types** and **non-nullable reference types**.
A nullable reference type is noted using the same syntax as nullable value types: a `?` is appended to the type of the variable:

```csharp
string? name;
```

Any variable where the `?` isn't appended to the type name is a non-nullable reference type. That includes all reference type variables in existing code when you've enabled this feature.

The compiler uses static analysis to determine if a nullable reference is known to be non-null. The compiler warns you if you dereference a nullable reference when it may be null. You can override this behavior by using the [null-forgiving operator ↑](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-forgiving) `!` following a variable name. For example, if you know the name variable isn't null but the compiler issues a warning, you can write the following code to override the compiler's analysis:

```csharp
name!.Length;
```

The null-forgiving operator has no effect at run time. It only affects the compiler's static flow analysis by changing the null state of the expression. At run time, expression `x!` evaluates to the result of the underlying expression `x`.

## Example

```csharp
using System.Diagnostics.CodeAnalysis;

class Program
{
    public static void Main()
    {
        A? a = Get<A>(2);
    }

    [return: MaybeNull]
    private static T Get<T>(int i) where T : new()
    {
        if (i > 10)
            return new T();

        return default;
    }
}

class A
{
}
```

## Links

- [↑ Nullable reference types](https://docs.microsoft.com/en-us/dotnet/csharp/nullable-references)
