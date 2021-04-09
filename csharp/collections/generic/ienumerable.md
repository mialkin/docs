# IEnumerable\<T> Interface

Exposes the enumerator, which supports a simple iteration over a collection of a specified type.

```csharp
IEnumerable<int> src = ...;
foreach (int item in src) Use(item);
```

This use of foreach is transformed by the compiler into calls on the underlying interfaces:

```bash
IEnumerable<int> src = ...;
IEnumerator<int> e = src.GetEnumerator();
try
{
  while (e.MoveNext()) Use(e.Current);
}
finally { if (e != null) e.Dispose(); }
```

## Links

* [↑ IEnumerable\<T> Interface](ienumerable.md)
* [↑ Iterating with Async Enumerables in C# 8](https://docs.microsoft.com/en-us/archive/msdn-magazine/2019/november/csharp-iterating-with-async-enumerables-in-csharp-8)