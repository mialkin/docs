# Returning from Task

With `async`:

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

Without `async`:

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
