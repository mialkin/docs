# Returning variable from Task

## With `async`

```csharp
using System.Collections.Generic;
using System.Threading.Tasks;

static async Task One()
{
    await Task.Delay(100);
}

static async Task<int> Two()
{
    // Cannot convert expression type 'System.Threading.Tasks.Task<int>' to return type 'int':
    // return Task.FromResult(5);

    await Task.Delay(100);
    return 5;
}

static async void Three()
{
    await Task.Delay(100);
}

static async IAsyncEnumerable<int> Four()
{
    for (int i = 0; i < 10; i++)
    {
        await Task.Delay(100);
        yield return i * 4;
    }
}
```

## Without `async`

```csharp
static Task A()
{
    // Creates a Task that will complete after a time delay
    // return Task.Delay(100);

    // Queues the specified work to run on the ThreadPool and returns a Task handle for that work.
    // return Task.Run(() => { });

    // Creates a Task that's completed due to cancellation with the specified token.
    // return Task.FromCanceled(new CancellationToken(true));

    // return Task.FromResult(10);

    return Task.CompletedTask;
}

static Task<int> B()
{
    // Cannot convert expression type 'int' to return type 'System.Threading.Tasks.Task<int>':
    // return 10;

    return Task.FromResult(10);
}
```

## Getting results back

Both `async` and non-`async` methods return `Task` types and both can be awaited:

```csharp
using System.Threading.Tasks;

Task<int> b = B();
int b1 = await B();

Task<int> c = C();
int c1 = await C();

static Task<int> B()
{
    return Task.FromResult(10);
}

static async Task<int> C()
{
    int result = await Task.FromResult(10);
    return result;
}
```

## Explanation

The `async` methods are different from normal methods. Whatever you return from an `async` method the result is wrapped by the compiler in a `Task`.

If you return no value, i.e. `void`, the "result" will be wrapped in a `Task`. If you return `int` the result will be wrapped in a `Task<int>`, and so on.

If your `async` method needs to return `int`, you'd mark the return type of the method as `Task<int>`, and you'll return plain `int` not `Task<int>`. Compiler will convert `int` to `Task<int>` for you:

```csharp
private async Task<int> MethodName()
{
    await SomethingAsync();
    return 42; // Note that we return int, not Task<int>, and that compiles.
}
```

In the same way, when you return `Task<object>` your method's return type should be `Task<Task<object>>`:

```csharp
public async Task<Task<object>> MethodName()
{
    return Task.FromResult<object>(null); // This will compile.
}
```

Since your method is returning `Task`, it shouldn't return any value. Otherwise it won't compile:

```csharp
public async Task MethodName()
{
    await SomethingAsync();
    return; // This will compile, but return is redundant.
}
```
