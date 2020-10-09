# Object.Equals

Determines whether two object instances are equal.

```csharp
using System.Runtime.CompilerServices;

namespace System
{
    public partial class Object
    {
        // Returns a boolean indicating if the passed in object obj is 
        // Equal to this.  Equality is defined as object equality for reference
        // types and bitwise equality for value types using a loader trick to
        // replace Equals with EqualsValue for value types).
        public virtual bool Equals(object? obj)
        {
            return RuntimeHelpers.Equals(this, obj);
        }

        public static bool Equals(object? objA, object? objB)
        {
            if (objA == objB)
            {
                return true;
            }
            if (objA == null || objB == null)
            {
                return false;
            }
            return objA.Equals(objB);
        }
    }
}
```

## Links

[↑ Object.Equals Method](https://docs.microsoft.com/en-us/dotnet/api/system.object.equals)
