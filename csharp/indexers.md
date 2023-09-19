# Indexers

Indexers allow instances of a class or struct to be indexed just like arrays.

```csharp
using System;

var stringCollection = new SampleCollection<string>();
stringCollection[0] = "Hello, World";
Console.WriteLine(stringCollection[0]); // => Hello, World.

class SampleCollection<T>
{
    // Declare an array to store the data elements.
    private readonly T[] _arr = new T[100];

    // Define the indexer to allow client code to use [] notation.
    public T this[int i]
    {
        get { return _arr[i]; }
        set { _arr[i] = value; }
    }
}
```

## Links

[↑ Indexers (C# Programming Guide)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/indexers/)
