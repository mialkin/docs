# Concurrent, immutable, generic collections

## Table of contents

- [Concurrent, immutable, generic collections](#concurrent-immutable-generic-collections)
  - [Table of contents](#table-of-contents)
  - [Concurrent collections](#concurrent-collections)
    - [`IProducerConsumerCollection<T>`](#iproducerconsumercollectiont)
      - [`ConcurrentBag<T>`](#concurrentbagt)
      - [`ConcurrentQueue<T>`](#concurrentqueuet)
      - [`ConcurrentStack<T>`](#concurrentstackt)
    - [`ConcurrentDictionary<TKey,TValue>`](#concurrentdictionarytkeytvalue)
    - [`BlockingCollection<T>`](#blockingcollectiont)
  - [Immutable collections](#immutable-collections)
    - [`ImmutableStack<T>`](#immutablestackt)
    - [`ImmutableList<T>`](#immutablelistt)
    - [`ImmutableArray<T>`](#immutablearrayt)
    - [`ImmutableDictionary<TKey, TValue>`](#immutabledictionarytkey-tvalue)
    - [Read-only vs immutable collections](#read-only-vs-immutable-collections)
  - [Generic collections](#generic-collections)
    - [`LinkedList<T>`](#linkedlistt)
    - [`SortedList<TKey,TValue>`](#sortedlisttkeytvalue)
    - [`SortedDictionary<TKey,TValue>`](#sorteddictionarytkeytvalue)
    - [`SortedSet`](#sortedset)

## Concurrent collections

The [↑ `System.Collections.Concurrent`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent) namespace includes several collection classes that are both thread-safe and scalable. Multiple threads can safely and efficiently add or remove items from these collections, without requiring additional synchronization in user code.

Some of the concurrent collection types use lightweight synchronization mechanisms such as [`SpinLock`](/csharp/concurrency/synchronization/locking.md#spinlock), [`SpinWait`](/csharp/concurrency/synchronization/locking.md#spinwait), [`SemaphoreSlim`](/csharp/concurrency/synchronization/locking.md#semaphoreslim), and [`CountdownEvent`](/csharp/concurrency/synchronization/signaling.md#countdownevent). These synchronization types typically use _busy spinning_ for brief periods before they put the thread into a true `Wait` state. When wait times are expected to be short, spinning is far less computationally expensive than waiting, which involves an expensive kernel transition. For collection classes that use spinning, this efficiency means that multiple threads can add and remove items at a high rate.

The [`ConcurrentQueue<T>`](#concurrentqueuet) and [`ConcurrentStack<T>`](#concurrentstackt) classes don't use locks at all. Instead, they rely on [`Interlocked`](/csharp/concurrency/synchronization/non-blocking.md#interlocked) operations to achieve thread safety.

The concurrent stack, queue, and bag classes are implemented internally with [linked lists](#linkedlistt). This makes them less memory-efficient than the nonconcurrent `Stack` and `Queue` classes, but better for concurrent access because linked lists are conducive to lock-free or low-lock implementations. This is because inserting a node into a linked list requires updating just a couple of references, while inserting an element into a `List<T>`-like structure may require moving thousands of existing elements.

The concurrent collections are tuned for parallel programming. The conventional collections outperform them in all but highly concurrent scenarios.

[↑ When to use a thread-safe collection](https://learn.microsoft.com/en-us/dotnet/standard/collections/thread-safe/when-to-use-a-thread-safe-collection).

### `IProducerConsumerCollection<T>`

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

#### `ConcurrentBag<T>`

The [↑ `ConcurrentBag<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentbag-1) class represents a thread-safe, unordered collection of objects.

Taking 1 element from the empty bag:

```csharp
var concurrentBag = new ConcurrentBag<int>();
var elements = concurrentBag.Take(1);
Console.WriteLine(elements.Count()); // 0
```

Counting elements in the bag:

```csharp
var concurrentBag = new ConcurrentBag<int>([1, 2, 2]);
Console.WriteLine(concurrentBag.Count); // 3
```

Taking 100 elements from the bag containing only 3 elements:

```csharp
var concurrentBag = new ConcurrentBag<int>([1, 2, 2]);
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

#### `ConcurrentQueue<T>`

The [↑ `ConcurrentQueue<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentqueue-1) class represents a thread-safe first in, first out, FIFO, collection.

```csharp
var concurrentQueue = new ConcurrentQueue<int>([1, 2, 3]);

concurrentQueue.Enqueue(4);
concurrentQueue.TryDequeue(out var dequeuedElement);
concurrentQueue.TryPeek(out var peekedElement);

Console.WriteLine($"Concurrent queue: {string.Join(", ", concurrentQueue)}. Is empty: {concurrentQueue.IsEmpty}");

// Output:
// Concurrent queue: 2, 3, 4. Is empty: False
```

#### `ConcurrentStack<T>`

The [↑ `ConcurrentStack<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentstack-1) class Represents a thread-safe last in, first out, LIFO, collection.

```csharp
var concurrentStack = new ConcurrentStack<int>([1, 2, 3, 4]);
concurrentStack.TryPop(out var poppedElement);
concurrentStack.Push(5);
Console.WriteLine($"Concurrent stack: {string.Join(", ", concurrentStack)}. Is empty: {concurrentStack.IsEmpty}");

// Output:
// Concurrent stack: 5, 3, 2, 1. Is empty: False
```

### `ConcurrentDictionary<TKey,TValue>`

The [↑ `ConcurrentDictionary<TKey,TValue>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentdictionary-2) class represents a thread-safe collection of key/value pairs that can be accessed by multiple threads concurrently.

```csharp
var concurrentDictionary = new ConcurrentDictionary<int, string>([
    new KeyValuePair<int, string>(1, "January"),
    new KeyValuePair<int, string>(2, "February"),
    new KeyValuePair<int, string>(3, "March"),
    new KeyValuePair<int, string>(4, "April")
]);

Console.WriteLine($"Concurrent dictionary contains 2: {concurrentDictionary.ContainsKey(2)}");
concurrentDictionary[5] = "May";
Console.WriteLine($"Concurrent dictionary: {string.Join(", ", concurrentDictionary)}");

// Output:
// Concurrent dictionary contains 2: True
// Concurrent dictionary: [1, January], [2, February], [3, March], [4, April], [5, May]
```

### `BlockingCollection<T>`

The [↑ `BlockingCollection<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.blockingcollection-1) provides blocking and bounding capabilities for thread-safe collections that implement [`IProducerConsumerCollection<T>`](#iproducerconsumercollectiont).

A blocking collection wraps any collection that implements `IProducerConsumerCollection<T>` and lets you `Take` an element from the wrapped collection — blocking if no element is available.

A blocking collection also lets you limit the total size of the collection, blocking the _producer_ if that size is exceeded. A collection limited in this manner is called a _bounded blocking collection_.

If you call the constructor without passing in a collection, the class will automatically instantiate a [`ConcurrentQueue<T>`](#concurrentqueuet). The producing and consuming methods let you specify cancellation tokens and timeouts. Add and `TryAdd` may block if the collection size is bounded; `Take` and `TryTake` block while the collection is empty.

Because we are not passing anything into `BlockingCollection`'s constructor, it instantiated a concurrent queue automatically:

```csharp
var blockingCollection = new BlockingCollection<int>(boundedCapacity: 3);

new Thread(() =>
{
    var value = 1;
    while (true)
    {
        Console.WriteLine($"Adding {value}...");
        // blockingCollection.TryAdd(value); // TryAdd method does not block
        blockingCollection.Add(value);
        Thread.Sleep(1000);
        value++;
    }
}).Start();

Thread.Sleep(10000);

new Thread(() =>
{
    while (true)
    {
      // blockingCollection.Take(); // Take method blocks
        blockingCollection.TryTake(out var element, 500);
        Console.WriteLine($"Took {element}");
    }
}).Start();

// Output:
// Adding 1...
// Adding 2...
// Adding 3...
// Adding 4...
// Took 1
// Took 2
// Took 3
// Took 4
// Took 0
// Adding 5...
// Took 5
// Took 0
// Adding 6...
// Took 6
// Took 0
```

Creating producer/consumer stack by passing `ConcurrentStack<T>` into constructor:

```csharp
var concurrentStack = new ConcurrentStack<int>([1, 2, 3]);
var blockingCollection = new BlockingCollection<int>(concurrentStack, 5);

new Thread(() =>
{
    var value = 4;
    while (true)
    {
        Console.WriteLine($"Adding {value}...");
        blockingCollection.Add(value);
        Thread.Sleep(1000);
        value++;
    }
}).Start();

Thread.Sleep(5000);

new Thread(() =>
{
    while (true)
    {
        var element = blockingCollection.Take();
        Console.WriteLine($"Took {element}");
    }
}).Start();

// Output:
// Adding 4...
// Adding 5...
// Adding 6...
// Took 5
// Took 6
// Took 4
// Took 3
// Took 2
// Took 1
// Adding 7...
// Took 7
// Adding 8...
// Took 8
```

`BlockingCollection` also provides static methods called `AddToAny` and `TakeFromAny`, which let you add or take an element while specifying several blocking collections. The action is then honored by the first collection able to service the request.

```csharp
var blockingCollection1 = new BlockingCollection<int>();
var blockingCollection2 = new BlockingCollection<int>();

new Thread(() =>
{
    var value = 1;
    while (true)
    {
        Console.WriteLine($"Adding {value}");
        blockingCollection1.Add(value);
        Thread.Sleep(7000);
        value++;
    }
}).Start();

new Thread(() =>
{
    var value = -1;
    while (true)
    {
        Console.WriteLine($"Adding {value}");
        blockingCollection2.Add(value);
        Thread.Sleep(3000);
        value--;
    }
}).Start();

new Thread(() =>
{
    while (true)
    {
        BlockingCollection<int>.TakeFromAny([blockingCollection1, blockingCollection2], out var element);
        Console.WriteLine($"Took {element}");
    }
}).Start();

// Output:
// Adding 1
// Adding -1
// Took 1
// Took -1
// Adding -2
// Took -2
// Adding -3
// Took -3
// Adding 2
// Took 2
// Adding -4
// Took -4
// Adding -5
// Took -5
```

## Immutable collections

Most immutable collections are [↑ implemented](https://learn.microsoft.com/en-us/archive/msdn-magazine/2017/march/net-framework-immutable-collections#immutable-lists) using balanced binary trees. The rest use [linked lists](concurrent-collections.md#linkedlistt) and only one, namely the [immutable array](#immutablearrayt), uses arrays.

Almost every mutable collection has a corresponding immutable one, which you can find in the [↑ `System.Collections.Immutable`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.immutable) namespace.

### `ImmutableStack<T>`

The [↑ `ImmutableStack<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.immutable.immutablestack-1) class represents an immutable stack.

```csharp
var immutableStack = ImmutableStack.Create(1, 2, 3, 4);
Console.WriteLine($"Immutable stack: {string.Join(",", immutableStack)}");

var secondStack = immutableStack.Pop(out var poppedElement);
Console.WriteLine($"Popped element from immutable stack: {poppedElement}");
Console.WriteLine($"Immutable stack after popping element: {string.Join(",", immutableStack)}");
Console.WriteLine($"Second stack formed by popping element from immutable stack: {string.Join(",", secondStack)}");

var peekedElement = secondStack.Peek();
Console.WriteLine($"Peeked element from second stack: {peekedElement}");

var thirdStack = secondStack.Push(5);
Console.WriteLine($"Third stack formed by pushing element to second stack: {string.Join(",", thirdStack)}");

var fourthStack = immutableStack.Clear();
Console.WriteLine($"Fourth stack formed by clearing immutable stack: {string.Join(",", fourthStack)}. " +
                  $"Is empty: {fourthStack.IsEmpty}");
Console.WriteLine($"Immutable stack: {string.Join(",", immutableStack)}. Is empty: {immutableStack.IsEmpty}");

// Output:
// Immutable stack: 4,3,2,1
// Popped element from immutable stack: 4
// Immutable stack after popping element: 4,3,2,1
// Second stack formed by popping element from immutable stack: 3,2,1
// Peeked element from second stack: 3
// Third stack formed by pushing element to second stack: 5,3,2,1
// Fourth stack formed by clearing immutable stack: . Is empty: True
// Immutable stack: 4,3,2,1. Is empty: False
```

### `ImmutableList<T>`

The [↑ `ImmutableList<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.immutable.immutablelist-1) class represents an immutable list, which is a strongly typed list of objects that can be accessed by index.

```csharp
var immutableList = ImmutableList.Create(1, 2, 3, 4);
var secondImmutableList = immutableList.Add(5);
var thirdImmutableList = secondImmutableList.Remove(3);

Console.WriteLine($"Immutable list: {string.Join(", ", immutableList)}");
Console.WriteLine($"Second immutable list: {string.Join(", ", secondImmutableList)}");
Console.WriteLine($"Third immutable list: {string.Join(", ", thirdImmutableList)}");

// Output:
// Immutable list: 1, 2, 3, 4
// Second immutable list: 1, 2, 3, 4, 5
// Third immutable list: 1, 2, 4, 5
```

### `ImmutableArray<T>`

The [↑ `ImmutableArray<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.immutable.immutablearray-1) structure represents an array that is immutable; meaning it cannot be changed once it is created.

There are different scenarios best for `ImmutableArray<T>` and others best for [`ImmutableList<T>`](#immutablelistt).

Reasons to use immutable array:

- Updating the data is rare or the number of elements is quite small (less than 16 items)
- You need to be able to iterate over the data in performance critical sections
- You have many instances of immutable collections and you can't afford keeping the data in trees

Reasons to use immutable list:

- Updating the data is common or the number of elements isn't expected to be small
- Updating the collection is more performance critical than iterating the contents

The following table summarizes the performance characteristics:

| Operation           | Get by index | Add      |
| ------------------- | ------------ | -------- |
| `ImmutableArray<T>` | O(1)         | O(n)     |
| `ImmutableList<T>`  | O(log n)     | O(log n) |

```csharp
var immutableArray = ImmutableArray.Create(1, 2, 3, 4);
var secondImmutableArray = immutableArray.Add(5);
var thirdImmutableArray = secondImmutableArray.Remove(3);

Console.WriteLine($"Immutable array: {string.Join(", ", immutableArray)}");
Console.WriteLine($"Second immutable array: {string.Join(", ", secondImmutableArray)}");
Console.WriteLine($"Third immutable array: {string.Join(", ", thirdImmutableArray)}");

// Output:
// Immutable array: 1, 2, 3, 4
// Second immutable array: 1, 2, 3, 4, 5
// Third immutable array: 1, 2, 4, 5
```

### `ImmutableDictionary<TKey, TValue>`

The [↑ `ImmutableDictionary<TKey, TValue>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.immutable.immutabledictionary-2) type uses a balanced binary tree to represent the dictionary. Each node in the tree contains an `ImmutableList<KeyValuePair<TKey, TValue>>`, which is also a balanced binary tree` that holds all the items that hash to the same value.

The `ImmutableDictionary<TKey, TValue>` is many times slower and consumes much more mem­ory than `Dictionary<TKey, TValue>`. You should definitely measure performance when using `ImmutableDictionary<TKey, TValue>` to determine whether it's acceptable or not.

### Read-only vs immutable collections

An instance of the [↑ `ReadOnlyCollection<T>`](https://stackoverflow.com/questions/30165810/why-use-immutablelist-over-readonlycollection) class is always read-only. A collection that is read-only is simply a collection with a wrapper that prevents modifying the collection; therefore, if changes are made to the underlying collection, the read-only collection reflects those changes.

```csharp
var list = new List<int> { 1, 2, 3 };

var readOnlyCollection = list.AsReadOnly();

list.Add(4);

Console.WriteLine(string.Join(", ", readOnlyCollection));

new Thread(() =>
{
    Thread.Sleep(1000);
    list.Add(5);
}).Start();

new Thread(() =>
{
    foreach (var item in readOnlyCollection)
    {
        Console.WriteLine(item);
        Thread.Sleep(1000);
    }
}).Start();

// Output:
// 1, 2, 3, 4
// 1
// Unhandled exception. System.InvalidOperationException: Collection was modified; enumeration operation may not execute.
```

## Generic collections

### `LinkedList<T>`

The [↑ `LinkedList<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1) class represents a doubly linked list.

The `LinkedList<T>` is _not_ concurrent collection and presented here just for the reference.

```csharp
var linkedList = new LinkedList<int>();

linkedList.AddFirst(1);
linkedList.AddLast(100);

var linkedListNode = linkedList.First;
var newNode = linkedList.AddAfter(linkedListNode!, 2);
var one = newNode.Previous;
Console.WriteLine($"One: {one!.Value}");

var hundred = linkedList.Find(100);
var ninetyNine = linkedList.AddBefore(hundred!, 99);
Console.WriteLine($"Hundred: {ninetyNine.Next!.Value}");

Console.WriteLine($"Linked list: {string.Join(", ", linkedList)}");
```

### `SortedList<TKey,TValue>`

The [↑ SortedList<TKey,TValue>](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.sortedlist-2) class represents a collection of key/value pairs that are sorted by key based on the associated [`IComparer<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.icomparer-1) implementation.

```csharp
var sortedList = new SortedList<int, string>
{
    { 3, "March" },
    { 1, "January" },
    { 5, "May" },
    { -1, "None" },
    { 2, "February" }
};

sortedList.Add(4, "April");
sortedList.Remove(-1);
Console.WriteLine($"Sorted list: {string.Join(", ", sortedList)}");
// Output:
// Sorted list: [1, January], [2, February], [3, March], [4, April], [5, May]
```

### `SortedDictionary<TKey,TValue>`

The [↑ `SortedDictionary<TKey,TValue>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.sorteddictionary-2) class represents a collection of key/value pairs that are sorted on the key.

```csharp
var sortedDictionary = new SortedDictionary<int, string>
{
    { 3, "March" },
    { 1, "January" },
    { 5, "May" },
    { -1, "None" },
    { 2, "February" }
};

sortedDictionary.Add(4, "April");
sortedDictionary.Remove(-1);
Console.WriteLine($"Sorted dictionary: {string.Join(", ", sortedDictionary)}");

// Output:
// Sorted dictionary: [1, January], [2, February], [3, March], [4, April], [5, May]
```

### `SortedSet`

```csharp
var sortedSet = new SortedSet<int>
{
    3,
    1,
    5,
    -1,
    2
};
sortedSet.Add(4);
sortedSet.Remove(-1);

Console.WriteLine($"Sorted set: {string.Join(", ", sortedSet)}");
// Sorted set: 1, 2, 3, 4, 5
```
