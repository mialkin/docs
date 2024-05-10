# Value types

**Value types** and [**reference types**](../reference-types.md) are the two main categories of C# types.

A variable of a value type contains an instance of the type. This differs from a variable of a reference type, which contains a reference to an instance of the type.

If a value type contains a data member of a reference type, only the reference to the instance of the reference type is copied when a value type instance is copied. Both the copy and original value type instance have access to the same reference type instance.

A value type can be one of the two following kinds:

- a [structure type](../struct.md), which encapsulates data and related functionality
- an [enumeration type](enumeration-types.md), which is defined by a set of named constants and represents a choice or a combination of choices

A nullable value type `T?` represents all values of its underlying value type `T` and an additional `null` value.

## Links

[↑ Value types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-types).
