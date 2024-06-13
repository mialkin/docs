# `Task`, `ValueTask`

## Table of contents

- [`Task`, `ValueTask`](#task-valuetask)
  - [Table of contents](#table-of-contents)
  - [`Task`](#task)
  - [`ValueTask`](#valuetask)
  - [Restrictions](#restrictions)
  - [Links](#links)

## `Task`

The [↑ `Task`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.task) class represents an asynchronous operation.

The [↑ `Task<TResult>`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.task-1) class represents an asynchronous operation that can return a value.

## `ValueTask`

`ValueTask<TResult>` type reduces memory allocations if the result is commonly synchronous; for example, if the result can be read from an in-memory cache.

`ValueTask<TResult>` is not directly convertible to `Task<TResult>`, but it does follow the awaitable pattern, so you can directly `await` it.

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

Most of your methods should return `Task<TResult>`, since consuming `Task<TResult>` has fewer pitfalls than consuming `ValueTask<TResult>`.

## Restrictions

A `ValueTask` or `ValueTask<TResult>` may only be awaited once.

Synchronously getting results from a `ValueTask` or `ValueTask<TResult>` may only be done once, after the `ValueTask` has completed, and that same cannot be awaited or converted to a task.

When your code calls a method returning `ValueTask` or `ValueTask<TResult>`, it should either immediately `await` that `ValueTask` or immediately call `AsTask` to convert it to a `Task`. This simple guideline doesn't cover all the advanced scenarios, but most applications will never need to do more than that.

## Links

[↑ `ValueTask<TResult>` structure](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.valuetask-1).

[↑ Understanding the Whys, Whats, and Whens of ValueTask](https://devblogs.microsoft.com/dotnet/understanding-the-whys-whats-and-whens-of-valuetask/).
