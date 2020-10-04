# GetHashCode

```csharp
using System.Runtime.CompilerServices;

namespace System
{
    public partial class Object
    {
        // GetHashCode is intended to serve as a hash function for this object.
        // Based on the contents of the object, the hash function will return a suitable
        // value with a relatively random distribution over the various inputs.
        //
        // The default implementation returns the sync block index for this instance.
        // Calling it on the same object multiple times will return the same value, so
        // it will technically meet the needs of a hash function, but it's less than ideal.
        // Objects (& especially value classes) should override this method.
        public virtual int GetHashCode()
        {
            return RuntimeHelpers.GetHashCode(this);
        }
    }
}
```
