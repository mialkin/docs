# Object.ReferenceEquals(Object, Object) method

Determines whether the specified Object instances are the same instance.

```csharp
public static bool ReferenceEquals (object objA, object objB);
```

Returns `true` if `objA` is the same instance as `objB` or if both are `null`; otherwise, `false`.

## Example

```csharp
using System.Runtime.CompilerServices;

namespace System
{
    public partial class Object
    {
        public static bool ReferenceEquals(object? objA, object? objB)
        {
            return objA == objB;
        }
    }
}
```

## Remarks

Unlike the [Equals](Equals.md) method and the equality operator, the `ReferenceEquals` method cannot be overridden. Because of this, if you want to test two object references for equality and you are unsure about the implementation of the Equals method, you can call the `ReferenceEquals` method.

## Links

[↑ Object.ReferenceEquals(Object, Object) Method](https://docs.microsoft.com/en-us/dotnet/api/system.object.referenceequals)