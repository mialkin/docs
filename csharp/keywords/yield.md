# yield

When you use the `yield` contextual keyword in a statement, you indicate that the method, operator, or get accessor in which it appears is an iterator. Using `yield` to define an iterator removes the need for an explicit extra class (the class that holds the state for an enumeration, see `IEnumerator<T>` for an example) when you implement the `IEnumerable` and `IEnumerator` pattern for a custom collection type.

When you use `yiled return` the compiler will automatically generate a class that implements both `IEnumerable<T>` and `IEnumerator<T>` (in the same class).

The following example shows the two forms of the `yield` statement:

```csharp
yield return <expression>;
yield break;
```

You use a `yield return` statement to return each element one at a time.

The sequence returned from an iterator method can be consumed by using a `foreach` statement or LINQ query. Each iteration of the `foreach` loop calls the iterator method. When a `yield return` statement is reached in the iterator method, `expression` is returned, and the current location in code is retained. Execution is restarted from that location the next time that the iterator function is called.

You can use a `yield break` statement to end the iteration.

## Example

```csharp
using System;
using System.Collections.Generic;

class Example
{
    static void Main()
    {
        foreach (int value in CreateSimpleIterator())
        {
            Console.WriteLine(value);
        }
    }

    static IEnumerable<int> CreateSimpleIterator()
    {
        yield return 10;

        for (int i = 0; i < 3; i++)
        {
            yield return i;
        }

        yield return 20;
    }
}
```

Output:

```output
10
0
1
2
20
```

So far, this isn’t terribly exciting. You could change the method to create a `List<int>`, replace each yield return statement with a call to `Add()`, and then return the list at the end of the method. The loop output would be exactly the same, but it wouldn’t execute in the same way at all. The huge difference is that iterators are executed **lazily**.

## Links

* [↑ What concrete type does 'yield return' return?](https://stackoverflow.com/questions/3454395/what-concrete-type-does-yield-return-return)
* [↑ Proper use of 'yield return'](https://stackoverflow.com/questions/410026/proper-use-of-yield-return)
* [↑ yield (C# Reference)](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/yield)
