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

Before adding a new item to the dictionary the `GetHashCode` method of the item's key object is called to determine bucket's index of the hashtable that backs up the dictionary. `Equals` is called on the item's key object only if there is an item already in the bucket that corresponds to the `int` returned by `GetHashcode`, and it's called as many times as many items are already in that bucket.

Now `Equals` returns always `true`. If you would make it to always return `false`, then the code above will throw "*An item with the same key has already been added*" exception. One one to avoid this exception would be to use `dict[a2] = "two"` instead of `dict.Add(a2, "two")`, the former will overwrite existing value if key already exists in the dictionary.

Now if you will try to run this code:

```csharp
string str1 = dict[a1];
```

first it will call `GetHashcode` on `a1` key to figure out bucket's index. Then it will call `Equals` on `a1`'s and `a2`'s keys that are already in the bucket. Since `Equals` returns always `false`, then it will raise "The given key 'A' was not present in the dictionary" exception.

Override guidelines: https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/equality-operators
