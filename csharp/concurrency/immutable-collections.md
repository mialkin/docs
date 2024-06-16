# Immutable collections

## Table of contents

- [Immutable collections](#immutable-collections)
  - [Table of contents](#table-of-contents)
  - [`ImmutableStack<T>`](#immutablestackt)
  - [`ImmutableList<T>`](#immutablelistt)

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
