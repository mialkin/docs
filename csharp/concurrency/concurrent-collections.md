# Concurrent collections

The [↑ `System.Collections.Concurrent`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent) namespace includes several collection classes that are both thread-safe and scalable. Multiple threads can safely and efficiently add or remove items from these collections, without requiring additional synchronization in user code.

Some of the concurrent collection types use lightweight synchronization mechanisms such as [`SpinLock`](/csharp/concurrency/synchronization/locking.md#spinlock), [`SpinWait`](/csharp/concurrency/synchronization/locking.md#spinwait), [`SemaphoreSlim`](/csharp/concurrency/synchronization/locking.md#semaphoreslim), and [`CountdownEvent`](/csharp/concurrency/synchronization/signaling.md#countdownevent). These synchronization types typically use *busy spinning* for brief periods before they put the thread into a true `Wait` state. When wait times are expected to be short, spinning is far less computationally expensive than waiting, which involves an expensive kernel transition. For collection classes that use spinning, this efficiency means that multiple threads can add and remove items at a high rate.

The [`ConcurrentQueue<T>`](#concurrentqueuet) and [`ConcurrentStack<T>`](#concurrentstackt) classes don't use locks at all. Instead, they rely on `Interlocked` operations to achieve thread safety.

[↑ When to use a thread-safe collection](https://learn.microsoft.com/en-us/dotnet/standard/collections/thread-safe/when-to-use-a-thread-safe-collection).

## Table of contents

- [Concurrent collections](#concurrent-collections)
  - [Table of contents](#table-of-contents)
  - [`IProducerConsumerCollection<T>`](#iproducerconsumercollectiont)
    - [`ConcurrentBag<T>`](#concurrentbagt)
    - [`ConcurrentQueue<T>`](#concurrentqueuet)
    - [`ConcurrentStack<T>`](#concurrentstackt)
    - [`BlockingCollection<T>`](#blockingcollectiont)

## `IProducerConsumerCollection<T>`

The [↑ `IProducerConsumerCollection<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.iproducerconsumercollection-1) interface defines methods to manipulate thread-safe collections intended for producer/consumer usage.

```csharp
public interface IProducerConsumerCollection<T> : IEnumerable<T>, IEnumerable, ICollection
{
    void CopyTo(T[] array, int index);
    bool TryAdd(T item);
    bool TryTake(out T item);
    T[] ToArray();
}
```

This interface provides a unified representation for producer/consumer collections so that higher level abstractions such as [`BlockingCollection<T>`](#blockingcollectiont) can use the collection as the underlying storage mechanism.

### `ConcurrentBag<T>`

The [↑ `ConcurrentBag<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentbag-1) class represents a thread-safe, unordered collection of objects.

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

### `ConcurrentQueue<T>`

The [↑ `ConcurrentQueue<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentqueue-1) class represents a thread-safe first in, first out, FIFO, collection.

```csharp
var concurrentQueue = new ConcurrentQueue<int>(new[] { 1, 2, 3 });

concurrentQueue.Enqueue(4);
concurrentQueue.TryDequeue(out var dequeuedElement);
concurrentQueue.TryPeek(out var peekedElement);

Console.WriteLine($"Concurrent queue: {string.Join(", ", concurrentQueue)}. Is empty: {concurrentQueue.IsEmpty}");

// Output:
// Concurrent queue: 2, 3, 4. Is empty: False
```

### `ConcurrentStack<T>`

The [↑ `ConcurrentStack<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentstack-1) class Represents a thread-safe last in, first out, LIFO, collection.

```csharp
var concurrentStack = new ConcurrentStack<int>(new[] { 1, 2, 3, 4 });
concurrentStack.TryPop(out var poppedElement);
concurrentStack.Push(5);
Console.WriteLine($"Concurrent stack: {string.Join(", ", concurrentStack)}. Is empty: {concurrentStack.IsEmpty}");

// Output:
// 
```

### `BlockingCollection<T>`

The [↑ `BlockingCollection<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.blockingcollection-1) provides blocking and bounding capabilities for thread-safe collections that implement [`IProducerConsumerCollection<T>`](#iproducerconsumercollectiont).
