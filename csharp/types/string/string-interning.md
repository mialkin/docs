# String interning and `string.Empty`

A **string interning** is an optimization that ensures that only one string object is created by the CLR for all instances of identical string [literals](/csharp/literal.md) inside compilation unit.

Here's a curious program fragment:

```csharp
object obj = "Int32";
string str1 = "Int32";
string str2 = typeof(int).Name; // "Int32"

Console.WriteLine(obj == str1); // true
Console.WriteLine(str1 == str2);// true
Console.WriteLine(obj == str2); // false !
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

## Links

[↑ String interning and `String.Empty`](https://learn.microsoft.com/en-us/archive/blogs/ericlippert/string-interning-and-string-empty).
