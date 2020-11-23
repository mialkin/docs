# IAsyncEnumerable\<T> Interface

Exposes an enumerator that provides asynchronous iteration over values of a specified type.

```csahrp
public interface IAsyncEnumerable<out T>
```

## Example

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        IAsyncEnumerable<string> result = A();

        await foreach (string word in result)
        {
            Console.Write(DateTime.Now.ToUniversalTime() + " : ");
            Console.WriteLine(word);
        }
    }

    static async IAsyncEnumerable<string> A()
    {
        var words = new List<string> { "un", "deux", "trois", "quatre", "cinq" };

        foreach (string word in words)
        {
            await Task.Delay(1000);
            yield return word;
        }
    }
}
```

Output:

```output
11/20/2020 14:37:57 : un
11/20/2020 14:37:58 : deux
11/20/2020 14:37:59 : trois
11/20/2020 14:38:00 : quatre
11/20/2020 14:38:01 : cinq
```

## Links

[↑ IAsyncEnumerable\<T> Interface](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.iasyncenumerable-1*)
[↑ Iterating with Async Enumerables in C# 8](https://docs.microsoft.com/en-us/archive/msdn-magazine/2019/november/csharp-iterating-with-async-enumerables-in-csharp-8)
