# Immutable collections

Most immutable collections are [↑ implemented](https://learn.microsoft.com/en-us/archive/msdn-magazine/2017/march/net-framework-immutable-collections#immutable-lists) using balanced binary trees. The rest use linked lists and only one, namely the [immutable array](#immutablearrayt), uses arrays.

Almost every mutable collection has a corresponding immutable one, which you can find in the [↑ `System.Collections.Immutable`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.immutable) namespace.

## Table of contents

- [Immutable collections](#immutable-collections)
  - [Table of contents](#table-of-contents)
  - [`ImmutableStack<T>`](#immutablestackt)
  - [`ImmutableList<T>`](#immutablelistt)
  - [`ImmutableArray<T>`](#immutablearrayt)
  - [`ImmutableDictionary<TKey, TValue>`](#immutabledictionarytkey-tvalue)
  - [Read-only vs immutable collections](#read-only-vs-immutable-collections)

## `ImmutableStack<T>`

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

## `ImmutableList<T>`

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

## `ImmutableArray<T>`

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

## `ImmutableDictionary<TKey, TValue>`

The [↑ `ImmutableDictionary<TKey, TValue>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.immutable.immutabledictionary-2) type uses a balanced binary tree to represent the dictionary. Each node in the tree contains an `ImmutableList<KeyValuePair<TKey, TValue>>`, which is also a balanced binary tree` that holds all the items that hash to the same value.

The `ImmutableDictionary<TKey, TValue>` is many times slower and consumes much more mem­ory than `Dictionary<TKey, TValue>`. You should definitely measure performance when using `ImmutableDictionary<TKey, TValue>` to determine whether it's acceptable or not.

## Read-only vs immutable collections

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
