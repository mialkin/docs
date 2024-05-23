# `stackalloc`

A [↑ `stackalloc`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/stackalloc) expression allocates a block of memory on the stack. A stack-allocated memory block created during the method execution is automatically discarded when that method returns. You can't explicitly free the memory allocated with `stackalloc`. A stack allocated memory block isn't subject to garbage collection and doesn't have to be pinned with a [`fixed`](/csharp/memory/garbage-collector.md#fixed-keyword) statement.

You can assign the result of a `stackalloc` expression to a variable of one of the following types: [`Span<T>`, `ReadOnlySpan<T>`](/csharp/types/value-types/struct-types.md#spant-readonlyspant), [↑ pointer type](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/unsafe-code#pointer-types).

You don't have to use an [↑ unsafe](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/unsafe) context when you assign a stack allocated memory block to a `Span<T>` or `ReadOnlySpan<T>` variable:

```csharp
int length = 3;
Span<int> numbers = stackalloc int[length];
for (var i = 0; i < length; i++)
{
    numbers[i] = i;
}
```

You must use an `unsafe` context when you work with pointer types:

```csharp
unsafe
{
    int length = 3;
    int* numbers = stackalloc int[length];
    for (var i = 0; i < length; i++)
    {
        numbers[i] = i;
    }
}
```
