# yield

## Example

```charp
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        IEnumerable<string> result = A();

        foreach (string word in result)
        {
            Console.WriteLine(word);
        }
    }

    static IEnumerable<string> A()
    {
        yield return "une";
        yield return "deux";
        yield return "trois";
        yield return "quatre";
        yield return "cinq";
    }
}
```
