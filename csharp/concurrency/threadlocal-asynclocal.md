# `ThreadLocal<T>`, `AsyncLocal<T>`, `[ThreadStatic]`

## Table of contents

- [`ThreadLocal<T>`, `AsyncLocal<T>`, `[ThreadStatic]`](#threadlocalt-asynclocalt-threadstatic)
  - [Table of contents](#table-of-contents)
  - [`ThreadLocal<T>`](#threadlocalt)
  - [`AsyncLocal<T>`](#asynclocalt)
    - [Example](#example)
    - [Usage with ASP.NET middleware](#usage-with-aspnet-middleware)
  - [`[ThreadStatic]`](#threadstatic)

## `ThreadLocal<T>`

[↑ `ThreadLocal<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.threadlocal-1) class provides thread-local storage for both static and instance fields — and allows you to specify default values.

Here's how to create a `ThreadLocal<int>` with a default value of 3 for each thread:

```csharp
private static ThreadLocal<int> _x = new (() => 3);
```

`ThreadLocal` values are lazily evaluated: the factory function evaluates on the first call for each thread.

Using `ThreadLocal` with a static variable:

```csharp
new Thread(() =>
{
    Console.WriteLine(A.ThreadLocal.Value + " " + A.Regular);
    A.ThreadLocal.Value = 5;
    A.Regular = 5;
    Thread.Sleep(1000);
    Console.WriteLine(A.ThreadLocal.Value + " " + A.Regular);
    Thread.Sleep(4000);
    Console.WriteLine(A.ThreadLocal.Value + " " + A.Regular);
}).Start();

new Thread(() =>
{
    Thread.Sleep(2000);
    Console.WriteLine(A.ThreadLocal.Value + " " + A.Regular);
    A.ThreadLocal.Value = 10;
    A.Regular = 10;
    Thread.Sleep(1000);
    Console.WriteLine(A.ThreadLocal.Value + " " + A.Regular);
}).Start();


class A
{
    public static readonly ThreadLocal<int> ThreadLocal = new(() => 3);
    public static int Regular = 3;
}

// Output:
// 3 3
// 5 5
// 3 5
// 10 10
// 5 10
```

Using `ThreadLocal` with a non-static variable:

```csharp
var a = new A();

new Thread(() =>
{
    Console.WriteLine(a.ThreadLocal.Value + " " + a.Regular);
    a.ThreadLocal.Value = 5;
    a.Regular = 5;
    Thread.Sleep(1000);
    Console.WriteLine(a.ThreadLocal.Value + " " + a.Regular);
    Thread.Sleep(4000);
    Console.WriteLine(a.ThreadLocal.Value + " " + a.Regular);
}).Start();

new Thread(() =>
{
    Thread.Sleep(2000);
    Console.WriteLine(a.ThreadLocal.Value + " " + a.Regular);
    a.ThreadLocal.Value = 10;
    a.Regular = 10;
    Thread.Sleep(1000);
    Console.WriteLine(a.ThreadLocal.Value + " " + a.Regular);
}).Start();


class A
{
    public readonly ThreadLocal<int> ThreadLocal = new(() => 3);
    public int Regular = 3;
}

// Output:
// 3 3
// 5 5
// 3 5
// 10 10
// 5 10
```

Consider the problem of generating random numbers in a multithreaded environment. The `Random` class is not thread-safe, so we have to either lock around using `Random`, limiting concurrency, or generate a separate `Random` object for each thread. `ThreadLocal<T>` makes the latter easy:

```csharp
using var localRandom = new ThreadLocal<Random>(() => new Random(Guid.NewGuid().GetHashCode()));
```

The `ThreadLocal<T>` class implements `IDisposable`. Always call [↑ `Dispose`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.threadlocal-1.dispose) before you release your last reference to the `ThreadLocal<T>`. Otherwise, the resources it is using will not be freed until the garbage collector calls the `ThreadLocal<T>` object's `Finalize` method.

```csharp
using var threadLocal = new ThreadLocal<int>();
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

### Usage with ASP.NET middleware

```csharp
public static class UserIdenity
{
    public static AsyncLocal<string> Id { get; } = new();
}
```

```csharp
public class UserIdentityMiddleware
{
    private readonly RequestDelegate _next;

    public UserIdentityMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        var cultureQuery = context.Request.Query["userId"];
        if (!string.IsNullOrWhiteSpace(cultureQuery))
        {
            UserIdenity.Id.Value = cultureQuery.First()!;
        }

        await _next(context);
    }
}
```

In your business logic layer:

```csharp
logger.LogInformation($"User ID: {UserIdenity.Id.Value}. Delaying for 10 second");
await Task.Delay(10000, cancellationToken);
logger.LogInformation($"User ID after 10 seconds delay: {UserIdenity.Id.Value}");
```

Run 2 queries, one right after another, in separate web browser tabs:

```text
http://localhost:2300?userId=Nikolay
http://localhost:2300?userId=Aleksei
```

```console
[01:08:57 INF] User ID: Nikolay. Delaying for 10 second
[01:08:59 INF] User ID: Aleksei. Delaying for 10 second
[01:09:07 INF] User ID after 10 seconds delay: Nikolay
[01:09:09 INF] User ID after 10 seconds delay: Aleksei
```

[↑ Conveying Context with AsyncLocal](https://medium.com/@norm.bryar/conveying-context-with-asynclocal-91fa474a5b42).

## `[ThreadStatic]`

[↑ `ThreadStaticAttribute`](https://learn.microsoft.com/en-us/dotnet/api/system.threadstaticattribute) is a class that indicates that the value of a static field is unique for each thread.

[↑ ThreadStatic v.s. ThreadLocal<T>: is generic better than attribute?](https://stackoverflow.com/questions/18333885/threadstatic-v-s-threadlocalt-is-generic-better-than-attribute).
