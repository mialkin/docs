# Cancellation

- [Cancellation](#cancellation)
  - [`CancellationToken.IsCancellationRequested`](#cancellationtokeniscancellationrequested)
  - [Canceling due to timeouts](#canceling-due-to-timeouts)

Cancellation is a type of signal, with two different sides: a source that triggers the cancellation and a receiver that then responds to the cancellation. In .NET, the source is `CancellationTokenSource` and the receiver is `CancellationToken`.

Each `CancellationTokenSource` is independent from every other one unless you link them. The `Token` property
returns a `CancellationToken` for that source, and the `Cancel` method issues the actual cancellation request.

```csharp
CancellationTokenSource cts = new();
CancellationToken token = cts.Token;

Task write = Task.Run(() => Write(token));
Task read = Task.Run(() => Read(cts));

try
{
    await write;
}
catch (OperationCanceledException)
{
    Console.WriteLine("Canceled");
}
catch (Exception e)
{
    Console.WriteLine("Completed with error");
    throw;
}

async Task Write(CancellationToken cancellationToken)
{
    int i = 1;

    while (true)
    {
        Console.WriteLine(i++);
        await Task.Delay(2000, cancellationToken);
    }
}

void Read(CancellationTokenSource cancellationTokenSource)
{
    if (Console.ReadKey().Key == ConsoleKey.A)
    {
        cancellationTokenSource.Cancel();
    }
}
```

## `CancellationToken.IsCancellationRequested`

From [StackOverflow](https://stackoverflow.com/a/10134604/1833895):

**Q:** Just curious, is there a reason why none of the examples use `CancellationToken.IsCancellationRequested` and instead suggest throwing exceptions?

**A:** If a method takes a `CancellationToken`, then the expected behavior is to throw an `OperationCanceledException` if the operation is cancelled. That's the standard behavior. Any other behavior (like returning a sentinel value) would be unusual and need explicit documentation.

Another implementation of the `Write` method from above could be:

```csharp
async Task Write(CancellationToken cancellationToken)
{
    int i = 1;

    while (true)
    {
        Console.WriteLine(i++);
        cancellationToken.ThrowIfCancellationRequested();
        await Task.Delay(100);
    }
}
```

If your loop is very tight (i.e., if the body of your loop executes very quickly), then you may want to limit how often you check your cancellation token. As always, measure your performance before and after a change like this before deciding which way is best.

## Canceling due to timeouts

To execute code with a timeout, use `CancellationTokenSource` specifying delay in constructor:

```csharp
using System.Threading;
using System.Threading.Tasks;

CancellationTokenSource cts = new(TimeSpan.FromSeconds(5));
CancellationToken token = cts.Token;

Task write = Task.Run(() => Write(token));

try
{
    await write;
}
catch (OperationCanceledException)
{
    Console.WriteLine("Canceled.");
}
catch (Exception e)
{
    Console.WriteLine("Completed with error.");
    throw;
}

async Task Write(CancellationToken cancellationToken)
{
    await Task.Delay(TimeSpan.FromSeconds(10), cancellationToken);
    Console.WriteLine("Delay completed successfully.");
}
```

or use `CancelAfter` that works the same way:

```csharp
CancellationTokenSource cts = new();
CancellationToken token = cts.Token;
cts.CancelAfter(TimeSpan.FromSeconds(5));
```
