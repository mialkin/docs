# `WeakReference`

`WeakReference` type represents a weak reference, which references an object while still allowing that object to be reclaimed by garbage collection.

[↑ WeakReference Class](https://learn.microsoft.com/en-us/dotnet/api/system.weakreference?view=net-7.0).

```bash
dotnet add package Bogus
```

If an object is reclaimed for garbage collection, a new data object should be regenerated; otherwise, the object is available to access because of the weak reference.

```csharp
using Bogus;

var faker = new Faker();

var guids = new List<Guid>();
var cache = new Cache();

for (var i = 0; i < 5; i++)
{
    var id = Guid.NewGuid();
    guids.Add(id);
    cache.Add(key: id, value: faker.Address.StreetAddress(true));
}

guids.ForEach(x => Console.WriteLine(cache.Get(x)));
Console.WriteLine("====Before collection====");

GC.Collect();

guids.ForEach(x => Console.WriteLine(cache.Get(x) ?? "Value is null"));
Console.WriteLine("====After collection====");

public class Cache
{
    private readonly Dictionary<Guid, WeakReference<string>> _cache = new();
    public void Add(Guid key, string value) => _cache.Add(key, new WeakReference<string>(value));
    public string? Get(Guid key)
    {
        var weakReference = _cache[key];

        if (weakReference.TryGetTarget(out var result))
            return result;
        
        return result;
    }
}
```

Output:

```console
070 Brook Rapid Apt. 584
199 Runte Hollow Suite 243
44636 Sauer Oval Suite 502
4162 Adams Manor Suite 685
954 Valerie Crossroad Apt. 719
====Before collection====
Value is null
Value is null
Value is null
Value is null
954 Valerie Crossroad Apt. 719
====After collection====
```

## Links

[↑ When should weak references be used?](https://stackoverflow.com/questions/1640889/when-should-weak-references-be-used)

[↑ When to use weak references in .Net?](https://softwareengineering.stackexchange.com/questions/185345/when-to-use-weak-references-in-net).
