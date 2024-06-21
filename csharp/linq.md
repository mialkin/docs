# LINQ

**Language-integrated query** or **LINQ** provides language-level querying capabilities, and a *higher-order function* API to C# and Visual Basic, that enable you to write expressive declarative code.

A **higher-order function** is a function that takes one or more functions as arguments and/or returns a function as its result.

## Deferred execution

Deferred execution means that the evaluation of an expression is delayed until its *realized* value is actually required.

```csharp
var i = 0;
var list = new List<int> { 1, 2, 3, 4, 5 };
var test = list.Where(x => x > 2).Select(_ => i++);

Console.WriteLine(i);

// Output:
// 0
```

By adding `ToList()`, or for example `First()`, after `Select(_ => i++)` the output becomes `3`.

Deferred execution can greatly improve performance when you have to manipulate large data collections, especially in programs that contain a series of chained queries or manipulations. In the best case, deferred execution enables only a single iteration through the source collection.

Deferred execution is supported directly in the C# language by the `yield` keyword ,in the form of the `yield/return` statement, when used within an iterator block.
