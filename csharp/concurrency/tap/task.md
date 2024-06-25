# `Task`, `ValueTask`, `TaskCompletionSource`

## Table of contents

- [`Task`, `ValueTask`, `TaskCompletionSource`](#task-valuetask-taskcompletionsource)
  - [Table of contents](#table-of-contents)
  - [`Task`](#task)
  - [`ValueTask`](#valuetask)
    - [Restrictions](#restrictions)
  - [`TaskCompletionSource`](#taskcompletionsource)

## `Task`

The [↑ `Task`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.task) class represents an asynchronous operation.

The [↑ `Task<TResult>`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.task-1) class represents an asynchronous operation that can return a value.

## `ValueTask`

The [↑ `ValueTask`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.valuetask) structure provides an awaitable result of an asynchronous operation.

The [↑ `ValueTask<TResult>`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.valuetask-1) stucture provides a value type that wraps a `Task<TResult>` and a `TResult`, only one of which is used.

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

### Restrictions

A `ValueTask` or `ValueTask<TResult>` may only be awaited once.

Synchronously getting results from a `ValueTask` or `ValueTask<TResult>` may only be done once, after the `ValueTask` has completed, and that same cannot be awaited or converted to a task.

When your code calls a method returning `ValueTask` or `ValueTask<TResult>`, it should either immediately `await` that `ValueTask` or immediately call `AsTask` to convert it to a `Task`. This simple guideline doesn't cover all the advanced scenarios, but most applications will never need to do more than that.

[↑ `ValueTask<TResult>` structure](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.valuetask-1).

[↑ Understanding the Whys, Whats, and Whens of ValueTask](https://devblogs.microsoft.com/dotnet/understanding-the-whys-whats-and-whens-of-valuetask/).

## `TaskCompletionSource`

The [↑ `TaskCompletionSource`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskcompletionsource) class represents the producer side of a `Task` unbound to a delegate, providing access to the consumer side through the `Task` property.

The [↑ `TaskCompletionSource<TResult>`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskcompletionsource-1) class represents the producer side of a `Task<TResult>` unbound to a delegate, providing access to the consumer side through the `Task` property.

The `Task` class achieves two distinct things:

- It schedules a delegate to run on a pooled thread
- It offers a rich set of features for managing work items: continuations, child tasks, exception marshaling, etc

Interestingly, these two things are not joined at the hip: you can leverage a task's features for managing work items without scheduling anything to run on the thread pool. The `TaskCompletionSource` enables this pattern of use.

To use `TaskCompletionSource` you simply instantiate the class. It exposes a Task property that returns a task upon which you can wait and attach continuations — just like any other task.

```csharp
var source = new TaskCompletionSource<int>();

new Thread(() =>
{
    Thread.Sleep(5000);
    source.SetResult(123);
}).Start();

var result = await source.Task;
Console.WriteLine(result);
// Output
// 123
```
