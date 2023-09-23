# `WeakReference`

`WeakReference` type represents a weak reference, which references an object while still allowing that object to be reclaimed by garbage collection.

[↑ WeakReference Class](https://learn.microsoft.com/en-us/dotnet/api/system.weakreference?view=net-7.0).

```bash
dotnet add package Bogus
```

If an object is reclaimed for garbage collection, a new data object should be regenerated; otherwise, the object is available to access because of the weak reference.

```csharp
using Bogus;

var ids = new List<Guid>();
var faker = new Faker();
var cache = new Cache();

for (var i = 0; i < 10; i++)
{
    var id = Guid.NewGuid();
    ids.Add(id);
    cache.Add(id, faker.Address.StreetAddress(true));
}

GC.Collect(0);

foreach (var id in ids)
{
    var value = cache.Get(id);
    Console.WriteLine(value ?? "Value is null");
}

Console.ReadLine();

public class Cache
{
    private readonly Dictionary<Guid, WeakReference<string>> _cache = new();

    public void Add(Guid key, string value)
    {
        _cache.Add(key, new WeakReference<string>(value));
    }

    public string? Get(Guid key)
    {
        var weakReference = _cache[key];

        if (weakReference.TryGetTarget(out var result))
            return result;
        
        return result;
    }
}
```
