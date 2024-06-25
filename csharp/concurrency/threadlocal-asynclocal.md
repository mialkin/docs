# `[ThreadStatic]`, `ThreadLocal<T>`, `AsyncLocal<T>`, `Lazy<T>`

## Table of contents

- [`[ThreadStatic]`, `ThreadLocal<T>`, `AsyncLocal<T>`, `Lazy<T>`](#threadstatic-threadlocalt-asynclocalt-lazyt)
  - [Table of contents](#table-of-contents)
  - [`[ThreadStatic]`](#threadstatic)
  - [`ThreadLocal<T>`](#threadlocalt)
  - [`AsyncLocal<T>`](#asynclocalt)
    - [Usage with ASP.NET middleware](#usage-with-aspnet-middleware)
  - [`Lazy<T>`](#lazyt)

## `[ThreadStatic]`

[↑ `ThreadStaticAttribute`](https://learn.microsoft.com/en-us/dotnet/api/system.threadstaticattribute) is a class that indicates that the value of a static field is unique for each thread.

The easiest approach to thread-local storage is to mark a static field with the `ThreadStatic` attribute:

```csharp
new Thread(() =>
{
    Console.WriteLine("T1: " + A.Name);
    A.Name = "John";
    Console.WriteLine("T1: " + A.Name);
}).Start();

new Thread(() =>
{
    Console.WriteLine("T2: " + A.Name);
    A.Name = "Bob";
    Console.WriteLine("T2: " + A.Name);
}).Start();

class A
{
    // Thread static field has initializer: it will be evaluated only once by the first thread (while invoking static
    // constructor of the containing type), other threads will observe default value of the field
    [ThreadStatic] public static string Name = "Mark";
}

// Output:
// T1: Mark
// T1: John
// T2: 
// T2: Bob
```

Unfortunately, `[ThreadStatic]` doesn't work with instance fields, it simply does nothing,; nor does it play well with field initializers — they execute only *once* on the thread that's running when the static constructor executes. If you need to work with instance fields — or start with a non-default value — [`ThreadLocal<T>`](#threadlocalt) provides a better option.

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

Ambient data, represented by `AsyncLocal<T>`, flows down the asynchronous call stack, not the opposite:

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

The `AsyncLocal<T>` allows to avoid [↑ tramp data](https://softwareengineering.stackexchange.com/questions/335005/is-there-a-name-for-the-anti-pattern-of-passing-parameters-that-will-only-be/335013#335013) code smell.

[↑ Conveying Context with AsyncLocal](https://medium.com/@norm.bryar/conveying-context-with-asynclocal-91fa474a5b42).

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
        var userId = context.Request.Query["userId"];
        if (!string.IsNullOrWhiteSpace(userId))
        {
            UserIdenity.Id.Value = userId.First()!;
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
http://localhost:2300?userId=Tom
http://localhost:2300?userId=Bob
```

```console
[01:08:57 INF] User ID: Tom. Delaying for 10 second
[01:08:59 INF] User ID: Bob. Delaying for 10 second
[01:09:07 INF] User ID after 10 seconds delay: Tom
[01:09:09 INF] User ID after 10 seconds delay: Bob
```

## `Lazy<T>`

The [↑ `Lazy<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.lazy-1) class provides support for lazy initialization.

```csharp
var a = new A();
var expensive = a.Expensive;

class A
{
    private readonly Lazy<Expensive> _expensive = new(valueFactory: () => new Expensive(), isThreadSafe: true);

    public Expensive Expensive => _expensive.Value;
}

class Expensive
{
}
```

If you pass `false` into `Lazy<T>`'s constructor, it implements the thread-unsafe lazy initialization pattern that we described at the start of this section — this makes sense when you want to use `Lazy<T>` in a single-threaded context.
