# Indexers

[↑ Indexers](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/indexers/) allow instances of a class or a structure to be indexed just like arrays.

```csharp
var stringCollection = new SampleCollection<string>
{
    [0] = "Hello, "
};

stringCollection[1] = "World!";

Console.Write(stringCollection[0]);
Console.WriteLine(stringCollection[1]);

class SampleCollection<T>
{
    // Declare an array to store the data element
    private readonly T[] _array = new T[100];

    // Define the indexer to allow client code to use [] notation
    public T this[int i]
    {
        get => _array[i];
        set => _array[i] = value;
    }
}

// Output:
// Hello, World!
```
