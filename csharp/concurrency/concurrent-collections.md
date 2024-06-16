# Concurrent collections

## `ConcurrentBag`

The [↑ `ConcurrentBag`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentbag-1) class represents a thread-safe, unordered collection of objects.

Taking 1 element from the empty bag:

```csharp
var concurrentBag = new ConcurrentBag<int>();
var elements = concurrentBag.Take(1);
Console.WriteLine(elements.Count()); // 0
```

Counting elements in the bag:

```csharp
var concurrentBag = new ConcurrentBag<int>(new[] { 1, 2, 2 });
Console.WriteLine(concurrentBag.Count); // 3
```

Taking 100 elements from the bag containing only 3 elements:

```csharp
var concurrentBag = new ConcurrentBag<int>(new[] { 1, 2, 2 });
var hundred = concurrentBag.Take(100);
Console.WriteLine(hundred.Count()); // 3
```

Adding and enumerating elements in parallel using 2 threads:

```csharp
using System.Collections.Concurrent;

var concurrentBag = new ConcurrentBag<int>();

Console.WriteLine("Start");

new Thread(() =>
{
    while (true)
    {
        concurrentBag.Add(Random.Shared.Next(100));
        Thread.Sleep(10);
    }
}).Start();

new Thread(() =>
{
    while (true)
    {
        Console.WriteLine("Start enumerating concurrent bag");
        
        foreach (var item in concurrentBag)
        {
            Console.WriteLine(item);
        }
        
        Console.WriteLine("Stop enumerating concurrent bag");
        Thread.Sleep(6);
    }
}).Start();

new Thread(() =>
{
    Thread.Sleep(100);
    Console.WriteLine("End");
    Environment.Exit(0);
}).Start();

// Output:
// Start
// Start enumerating concurrent bag
// Stop enumerating concurrent bag
// Start enumerating concurrent bag
// 34
// Stop enumerating concurrent bag
// Start enumerating concurrent bag
// 9
// 34
// Stop enumerating concurrent bag
// Start enumerating concurrent bag
// 63
// 9
// 34
// Stop enumerating concurrent bag
// ...
// ...
// End
```