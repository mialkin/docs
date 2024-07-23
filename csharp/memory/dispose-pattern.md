# Unmanaged resource, `IDisposable`, dispose pattern, `WeakReference`

## Table of contents

- [Unmanaged resource, `IDisposable`, dispose pattern, `WeakReference`](#unmanaged-resource-idisposable-dispose-pattern-weakreference)
  - [Table of contents](#table-of-contents)
  - [Unmanaged resource](#unmanaged-resource)
  - [`IDisposable`, dispose pattern](#idisposable-dispose-pattern)
  - [`WeakReference`](#weakreference)

## Unmanaged resource

An [↑ unmanaged resource](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/unmanaged) is an object that wraps operating system resources, such as files, windows, network connections, or database connections.

Although the garbage collector is able to track the lifetime of an object that encapsulates an unmanaged resource, it doesn't know how to release and clean up the unmanaged resource.

[↑ What exactly are unmanaged resources?](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/unmanaged).

## `IDisposable`, dispose pattern

The [↑ `IDisposable`](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable) interface provides a mechanism for releasing [unmanaged resources](#unmanaged-resource).

> It is possible for a base class to only reference managed objects, and implement the dispose pattern. In these cases, a finalizer is unnecessary. A finalizer is only required if you directly reference unmanaged resources.

Here's an example of the general pattern for implementing the dispose pattern for a base class that uses a safe handle:

```csharp
using Microsoft.Win32.SafeHandles;
using System;
using System.Runtime.InteropServices;

class BaseClassWithSafeHandle : IDisposable
{
    // To detect redundant calls
    private bool _disposed;

    // Instantiate a SafeHandle instance.
    private SafeHandle _safeHandle = new SafeFileHandle(IntPtr.Zero, true);

    // Public implementation of Dispose pattern callable by consumers.
    public void Dispose() => Dispose(true);

    // Protected implementation of Dispose pattern.
    protected virtual void Dispose(bool disposing)
    {
        if (!_disposed)
        {
            if (disposing)
            {
                _safeHandle.Dispose();
            }

            _disposed = true;
        }
    }
}
```

[↑ Implement a `Dispose` method](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-dispose).

> The previous example uses a `SafeFileHandle` object to illustrate the pattern; any object derived from `SafeHandle` could be used instead.

Here's the general pattern for implementing the dispose pattern for a base class that overrides `Object.Finalize`.

```csharp
using System;

class BaseClassWithFinalizer : IDisposable
{
    private bool _disposed;

    ~BaseClassWithFinalizer() => Dispose(false);

    // Public implementation of Dispose pattern callable by consumers.
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    // Protected implementation of Dispose pattern.
    protected virtual void Dispose(bool disposing)
    {
        if (!_disposed)
        {
            if (disposing)
            {
                // TODO: dispose managed state (managed objects)
            }

            // TODO: free unmanaged resources (unmanaged objects) and override finalizer
            // TODO: set large fields to null
            _disposed = true;
        }
    }
}
```

## `WeakReference`

The [`WeakReference`](https://learn.microsoft.com/en-us/dotnet/api/system.weakreference) class represents a weak reference, which references an object while still allowing that object to be reclaimed by garbage collection.

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

// Output:

// 070 Brook Rapid Apt. 584
// 199 Runte Hollow Suite 243
// 44636 Sauer Oval Suite 502
// 4162 Adams Manor Suite 685
// 954 Valerie Crossroad Apt. 719
// ====Before collection====
// Value is null
// Value is null
// Value is null
// Value is null
// 954 Valerie Crossroad Apt. 719
```

[↑ When should weak references be used?](https://stackoverflow.com/questions/1640889/when-should-weak-references-be-used)

[↑ When to use weak references in .Net?](https://softwareengineering.stackexchange.com/questions/185345/when-to-use-weak-references-in-net).
