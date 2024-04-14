# `ThreadLocal<T>`, `AsyncLocal<T>`, `[ThreadStatic]`

## Table of contents

- [`ThreadLocal<T>`, `AsyncLocal<T>`, `[ThreadStatic]`](#threadlocalt-asynclocalt-threadstatic)
  - [Table of contents](#table-of-contents)
  - [`ThreadLocal<T>`](#threadlocalt)
  - [`AsyncLocal<T>`](#asynclocalt)
  - [`[ThreadStatic]`](#threadstatic)

## `ThreadLocal<T>`

[↑ `ThreadLocal<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.threadlocal-1) class provides thread-local storage of data.

```csharp
public class ThreadLocal<T> : IDisposable
```

Example:

```csharp
// Thread-Local variable that yields an ID for a thread
var threadId = new ThreadLocal<int>(valueFactory: () => Environment.CurrentManagedThreadId);

// Action that prints out thread ID for the current thread
var action = () =>
{
    // If ThreadName.IsValueCreated is true, it means that we are not the
    // first action to run on this thread
    Console.WriteLine($"IsValueCreated: {threadId.IsValueCreated}. Thread ID: {threadId.Value}");
};

// Launch 8 of them. On 4 cores or less, you should see some repeat thread
Parallel.Invoke(action, action, action, action, action, action, action, action);

// Dispose when you are done
threadId.Dispose();

// Output:
// IsValueCreated: False. Thread ID: 7
// IsValueCreated: True. Thread ID: 7
// IsValueCreated: False. Thread ID: 4
// IsValueCreated: True. Thread ID: 7
// IsValueCreated: True. Thread ID: 4
// IsValueCreated: False. Thread ID: 9
// IsValueCreated: False. Thread ID: 15
// IsValueCreated: False. Thread ID: 14
```

## `AsyncLocal<T>`

[↑ `AsyncLocal<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.asynclocal-1) class represents ambient data that is local to a given asynchronous *control flow*, such as an asynchronous method.

A **flow of control** is the order in which individual statements, instructions or function calls are executed or evaluated.

The following example uses the `AsyncLocal<T>` class to persist a string value across an asynchronous flow. It also contrasts the use of `AsyncLocal<T>` with [`ThreadLocal<T>`](threadlocal.md).

## `[ThreadStatic]`

[↑ `ThreadStaticAttribute`](https://learn.microsoft.com/en-us/dotnet/api/system.threadstaticattribute) is a class that indicates that the value of a static field is unique for each thread.

[↑ ThreadStatic v.s. ThreadLocal<T>: is generic better than attribute?](https://stackoverflow.com/questions/18333885/threadstatic-v-s-threadlocalt-is-generic-better-than-attribute).
