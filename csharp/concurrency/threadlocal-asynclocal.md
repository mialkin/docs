# `ThreadLocal<T>`, `AsyncLocal<T>`, `[ThreadStatic]`

## Table of contents

- [`ThreadLocal<T>`, `AsyncLocal<T>`, `[ThreadStatic]`](#threadlocalt-asynclocalt-threadstatic)
  - [Table of contents](#table-of-contents)
  - [`ThreadLocal<T>`](#threadlocalt)
  - [`AsyncLocal<T>`](#asynclocalt)
    - [Example](#example)
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

Because the task-based asynchronous programming model tends to abstract the use of threads, `AsyncLocal<T>`instances can be used to persist data across threads.

The `AsyncLocal<T>` class also provides optional notifications when the value associated with the current thread changes, either because it was explicitly changed by setting the `Value` property, or implicitly changed when the thread encountered an await or other context transition.

### Example

By definition, `AsyncLocal` represents ambient data that is local to a given asynchronous control flow, such as an asynchronous method. However, this contextual data flows down the asynchronous call stack, not the opposite:

```csharp
AsyncLocal<string> context = new();
const string parentValue = "Parent";
const string childValue = "Child";

async Task ParentTaskAsync()
{
    context.Value = parentValue;
    Debug.Assert(parentValue == context.Value);

    await ChildTaskAsync();
    Debug.Assert(parentValue == context.Value);
}

async Task ChildTaskAsync()
{
    Debug.Assert(parentValue == context.Value);
    context.Value = childValue;
    await Task.Yield();
    Debug.Assert(childValue == context.Value);
}

await ParentTaskAsync();
```

However, if we use a non-async child task, it behaves just like a normal synchronous method:

```csharp
AsyncLocal<string> context = new();
const string parentValue = "Parent";
const string childValue = "Child";

async Task ParentTaskAsync()
{
    context.Value = parentValue;
    Debug.Assert(parentValue == context.Value);

    await ChildTaskAsync();
    Debug.Assert(childValue == context.Value);
}

Task ChildTaskAsync()
{
    Debug.Assert(parentValue == context.Value);
    context.Value = childValue;
    Debug.Assert(childValue == context.Value);

    return Task.CompletedTask;
}

await ParentTaskAsync();
```

This time, value changes inside the child affect the parent `Task` as well.

[↑ Difference Between Returning and Awaiting a Task in C#](https://code-maze.com/charp-difference-between-returning-and-awaiting-a-task/).

## `[ThreadStatic]`

[↑ `ThreadStaticAttribute`](https://learn.microsoft.com/en-us/dotnet/api/system.threadstaticattribute) is a class that indicates that the value of a static field is unique for each thread.

[↑ ThreadStatic v.s. ThreadLocal<T>: is generic better than attribute?](https://stackoverflow.com/questions/18333885/threadstatic-v-s-threadlocalt-is-generic-better-than-attribute).
