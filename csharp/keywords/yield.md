# yield

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

* [↑ Proper use of 'yield return'](https://stackoverflow.com/questions/410026/proper-use-of-yield-return)

* [↑ yield (C# Reference)](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/yield)
