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

So far, this isn't terribly exciting. You could change the method to create a `List<int>`, replace each yield return statement with a call to `Add()`, and then return the list at the end of the method. The loop output would be exactly the same, but it wouldn't execute in the same way at all. The huge difference is that iterators are executed **lazily**.

## Laziness

```csharp
IEnumerable<int> GetNumbers()
{
    Console.WriteLine("I am lazy");

    yield return 1;

    Console.WriteLine("Returned 1 before. About to return 2");

    yield return 2;

    Console.WriteLine("Returned 2 before. About to return 3");

    yield return 3;

    Console.WriteLine("Returned 3 before. The end");
}

var sum = 0;

var collection = GetNumbers();

Console.WriteLine("About to iterate through collection");

foreach (var number in collection)
{
    sum += number;
}

Console.WriteLine($"The sum: {sum}");
```

Output:

```output
About to iterate through collection
I am lazy
Returned 1 before. About to return 2
Returned 2 before. About to return 3
Returned 3 before. The end
The sum: 6
```

When you start to iterate over an iterator's result, an iterator is executed until the first `yield return` statement is reached. Then, the execution of an iterator is suspended and the caller gets the first iteration value and processes it. On each subsequent iteration, the execution of an iterator resumes after the `yield return` statement that caused the previous suspension and continues until the next `yield return` statement is reached. The iteration completes when control reaches the end of an iterator or a `yield break` statement.

```csharp
IEnumerable<int> GetNumbers()
{
    Console.WriteLine("I am lazy");

    for (var i = 0; i < 3; i++)
    {
        Console.WriteLine($"About to return {i}");
        yield return i;
        Console.WriteLine($"Returned {i} before");
    }
    
    Console.WriteLine("The end");
}

var sum = 0;

var collection = GetNumbers();

Console.WriteLine("About to iterate through collection");

foreach (var number in collection)
{
    sum += number;
}

Console.WriteLine($"The sum: {sum}");
```

Output:

```console
I am lazy
About to return 0
Returned 0 before
About to return 1
Returned 1 before
About to return 2
Returned 2 before
The end
The sum: 3
```

## Links

- [↑ What concrete type does 'yield return' return?](https://stackoverflow.com/questions/3454395/what-concrete-type-does-yield-return-return)
- [↑ Proper use of 'yield return'](https://stackoverflow.com/questions/410026/proper-use-of-yield-return)
- [↑ yield (C# Reference)](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/yield)
