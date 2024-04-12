# `IAsyncEnumerable<T>`

[↑ `IAsyncEnumerable<T>`](https://learn.microsoft.com/ru-ru/dotnet/api/system.collections.generic.iasyncenumerable-1) interface exposes an enumerator that provides asynchronous iteration over values of a specified type.

```csharp
public interface IAsyncEnumerable<out T>
{
    IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
}
```

```csharp
public interface IAsyncEnumerator<out T> : IAsyncDisposable
{
    ValueTask<bool> MoveNextAsync();
    T Current { get; }
}
```

```csharp
public interface IAsyncDisposable
{
    ValueTask DisposeAsync();
}
```

## Example

```csharp
await foreach (var number in GetNumbersAsync())
{
    Console.WriteLine(number);
}

static async IAsyncEnumerable<int> GetNumbersAsync()
{
    for (int i = 1; i <= 5; i++)
    {
        await Task.Delay(1000);
        yield return i;
    }
}
// Output:
// 1
// 2
// 3
// 4
// 5

```

## Links

[↑ Iterating with Async Enumerables in C# 8](https://docs.microsoft.com/en-us/archive/msdn-magazine/2019/november/csharp-iterating-with-async-enumerables-in-csharp-8).
