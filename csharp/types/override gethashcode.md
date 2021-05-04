# Why override `GetHashCode` when `Equals` is overriden

Below `GetHashCode` will be called twice: right before `a1` and `a2` are added into the dictionary; and `Equals` will be called once: before `a2` is added:

```csharp
using System;
using System.Collections.Generic;

A a1 = new();
A a2 = new();

var dict = new Dictionary<A, string>();

dict.Add(a1, "one");
dict.Add(a2, "two");

Console.WriteLine("End");

class A
{

    public override bool Equals(object? obj)
    {
        return false;
    }

    public override int GetHashCode()
    {
        return 12345;
    }
}
```

Before adding a new item to the dictionary the `GetHashCode` method of the item's key object is called to determine index of the bucket of the hashtable that backs up the dictionary. `Equals` is called on the item's key object only if there is an item already in the bucket that corresponds to the `int` returned by `GetHashcode`, and it's called as many times as many items are already in that bucket.
