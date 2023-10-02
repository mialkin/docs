# `dynamic`

The `dynamic` type is a *static* type, but an object of type `dynamic` bypasses static type checking.

C# is a [↑ statically-typed](https://en.wikipedia.org/wiki/Type_system#Static_typing), [↑ strongly-typed](https://en.wikipedia.org/wiki/Strong_and_weak_typing) language.

In most cases, an object of `dynamic` type functions like it has type `object`. The compiler assumes a dynamic element supports any operation. Therefore, you don't have to determine whether the object gets its value from a COM API, from a dynamic language such as IronPython, from the HTML Document Object Model, DOM, from reflection, or from somewhere else in the program. However, if the code isn't valid, errors surface at run time.

The `dynamic` type differs from `object` in that operations that contain expressions of type `dynamic` aren't resolved or type checked by the compiler. The compiler packages together information about the operation, and that information is later used to evaluate the operation at run time. As part of the process, variables of type `dynamic` are compiled into variables of type `object`. Therefore, type `dynamic` exists only at compile time, not at run time.

## Links

[↑ The dynamic type](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/reference-types#the-dynamic-type).

[↑ Using type dynamic](https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/interop/using-type-dynamic).
