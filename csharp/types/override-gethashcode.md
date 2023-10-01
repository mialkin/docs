# Override `GetHashCode` when `Equals` is overridden

When two objects are equal they _must_ return the same hash code, but reverse is not necessary: two object may return the same hash code, but they can be not equal.

If you override `Equals` method you must also override `GetHashCode` method, because if you don't than two objects which are in fact equal will likely end up residing in different buckets inside of `Dictionary<TKey,TValue>` of `HashSet<T>`. Therefore you will have multiple equal keys in your dictionary or hash table.

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

Before adding a new item to the dictionary the `GetHashCode` method of the item's key object is called to determine bucket's index of the hash table that backs up the dictionary. `Equals` is called on the item's key object only if there is an item already in the bucket that corresponds to the `int` returned by `GetHashCode`, and it's called as many times as many items are already in that bucket.

Now `Equals` returns always `true`. If you would make it to always return `false`, then the code above will throw "_An item with the same key has already been added_" exception. One way to avoid this exception would be to use `dict[a2] = "two"` instead of `dict.Add(a2, "two")`, the former will overwrite existing value if key already exists in the dictionary.

Now if you will try to run this code:

```csharp
string str1 = dict[a1];
```

first it will call `GetHashCode` on `a1` key to figure out bucket's index. Then it will call `Equals` on `a1`'s and `a2`'s keys that are already in the bucket. Since `Equals` returns always `false`, then it will raise "The given key 'A' was not present in the dictionary" exception.

Also take a look at [↑ `HashCode.Combine`](https://docs.microsoft.com/en-us/dotnet/api/system.hashcode.combine) method that combines several values into a single hash code.

## Links

- [↑ Equality Operators](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/equality-operators)
- [↑ How to define value equality for a class or struct](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type)
