# `Span<T>`

`Span<T>` type provides a type-safe and memory-safe representation of a contiguous region of arbitrary memory.

`Span<T>` is a [↑ `ref struct`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/ref-struct) that is allocated on the stack rather than on the managed heap.

A `Span<T>` represents a contiguous region of arbitrary memory. A `Span<T>` instance is often used to hold the elements of an array or a portion of an array. Unlike an array, however, a `Span<T>` instance can point to managed memory, native memory, or memory managed on the stack.

## Links

[↑ `Span<T>` structure](https://learn.microsoft.com/en-us/dotnet/api/system.span-1).

[↑ `Span<T>` on YouTube](https://www.youtube.com/results?search_query=nick+chapsas+span).
