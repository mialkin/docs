# Dispose pattern

[↑ Implement a Dispose method](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-dispose)

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
