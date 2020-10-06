# Nullable reference types

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

* [↑ Nullable reference types](https://docs.microsoft.com/en-us/dotnet/csharp/nullable-references)
* [↑ Reserved attributes contribute to the compiler's null state static analysis](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/attributes/nullable-analysis)
