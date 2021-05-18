# ValueTask\<TResult> struct

The Base Class Library includes the `ValueTask<T>` type, which reduces memory allocations if the result is commonly synchronous; for example, if the result can be read from an in-memory cache. `ValueTask<T>` is not directly convertible to `Task<T>`, but it does follow the awaitable pattern, so you can directly await it.

As such, the default choice for any asynchronous method should be to return a `Task` or `Task<TResult>`. Only if performance analysis proves it worthwhile should a `ValueTask<TResult>` be used instead of a `Task<TResult>`.

```csharp
public ValueTask<int> MethodAsync()
{
    if (CanBehaveSynchronously)
        return new ValueTask<int>(13);
    
    return new ValueTask<int>(SlowMethodAsync());
}

private Task<int> SlowMethodAsync();
```

The non generic version of `ValueTask` is not recommended for most scenarios.

Most of your methods should return `Task<T>`, since consuming `Task<T>` has fewer pitfalls than consuming `ValueTask<T>`.

[↑ ValueTask\<TResult> Struct](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.valuetask-1)
