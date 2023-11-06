# String interning and `string.Empty`

A **string interning** is a compiler optimization that ensures that only one string object is created by the CLR for all instances of identical string [literals](/csharp/literal.md) inside compilation unit.

Here's a curious program fragment:

```csharp
object obj = "Int32";
string str1 = "Int32";
string str2 = typeof(int).Name; // "Int32"

Console.WriteLine(obj == str1); // True
Console.WriteLine(str1 == str2); // True
Console.WriteLine(obj == str2); // False!

/*
Console.WriteLine(ReferenceEquals(obj, str1)); // True
Console.WriteLine(ReferenceEquals(str1, str2)); // False
Console.WriteLine(ReferenceEquals(obj, str2)); // False
*/
```

Surely if A equals B, and B equals C, then A equals C; that's the transitive property of equality. It appears to have been thoroughly violated here.

Well, first off, though the transitive property is desirable, this is just one of many situations in which equality is intransitive in C#. You shouldn't rely upon transitivity *in general*, though of course there are many specific cases where it is valid.

Second, what's happening here is we're mixing two different kinds of equality that just happen to use the same operator syntax. We're mixing *reference* equality with *value* equality. Objects are compared by reference; in the first and third comparison we are testing if the two object references both refer to exactly the same object. In the second comparison we are checking to see if the two strings have the same content, regardless of whether they are the same object or not. In fact, the compiler warns you about this situation; this should produce a "possible unintended reference comparison" warning:

```text
Possible unintended reference comparison. To get a value comparison, cast the left hand side to type 'string'.
```

In .NET you can have two strings that have identical content but are different objects. When you compare those strings as strings, they're equal, but when you compare them as objects, they're not.

The first comparison is `true` because the two literals get turned into the same string object.

The second comparison is `true` because it's a value comparison.

The third comparison is `false` because it's a reference comparison and the literal and the computed value are turned into different objects.

## `string.Empty`

`string.Empty` is not a constant, it's a read-only field in another assembly. Therefore it is not interned with the empty string in your assembly; those are two different objects.

```csharp
namespace System
{
    public sealed partial class String : IComparable, IEnumerable, IConvertible, IEnumerable<char>, IComparable<string?>, IEquatable<string?>, ICloneable
    {
        ...
        [Intrinsic]
        public static readonly string Empty;
        ...
    }
    ...
}
```

```csharp
namespace System.Runtime.CompilerServices
{
    // Calls to methods or references to fields marked with this attribute may be replaced at
    // some call sites with jit intrinsic expansions.
    // Types marked with this attribute may be specially treated by the runtime/compiler.
    [AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct | AttributeTargets.Method | AttributeTargets.Constructor | AttributeTargets.Field, Inherited = false)]
    internal sealed class IntrinsicAttribute : Attribute
    {
    }
}
```

Some versions of the .NET runtime automatically intern the empty string at runtime, some do not:

```csharp
object obj = "";
string str1 = "";
string str2 = string.Empty;

Console.WriteLine(obj == str1); // True
Console.WriteLine(str1 == str2); // True
Console.WriteLine(obj == str2); // Sometimes True, sometimes False!
```

## Intern pool

.NET has the concept of an "intern pool". It's basically just a set of strings, but it makes sure that every time you reference the same string literal, you get a reference to the same string.

As well as literals being automatically interned, you can intern strings manually with the [↑ `Intern`](https://learn.microsoft.com/en-us/dotnet/api/system.string.intern) method, and check whether or not there is already an interned string with the same character sequence in the pool using the [↑ `IsInterned`](https://learn.microsoft.com/en-us/dotnet/api/system.string.isinterned) method. This somewhat unintuitively returns a string rather than a boolean — if an equal string is in the pool, a reference to that string is returned. Otherwise, null is returned. Likewise, the `Intern` method returns a reference to an interned string — either the string you passed in if was already in the pool, or a newly created interned string, or an equal string which was already in the pool.

## Links

[↑ String interning and `String.Empty`](https://learn.microsoft.com/en-us/archive/blogs/ericlippert/string-interning-and-string-empty).
