# `IEnumerable<T>`

```csharp
using System;
using System.Collections.Generic;

var list = new List<string> {"Un", "Deux", "Trois"};

foreach (var item in list)
{
    Console.WriteLine(item);
}
```

[↑ ILSpy](https://github.com/icsharpcode/ILSpy) or <https://sharplab.io> gives this output for the code above:

```csharp
// Warning: Some assembly references could not be resolved automatically. This might lead to incorrect decompilation of some parts,
// for ex. property getter/setter access. To get optimal decompilation results, please manually add the missing references to the list of loaded assemblies.
// <Program>$
using System;
using System.Collections.Generic;

private static void <Main>$(string[] args)
{
  //IL_002c: Unknown result type (might be due to invalid IL or missing references)
  //IL_0031: Unknown result type (might be due to invalid IL or missing references)
  List<string> obj = new List<string>();
  obj.Add("Un");
  obj.Add("Deux");
  obj.Add("Trois");
  List<string> list = obj;
  Enumerator<string> enumerator = list.GetEnumerator();
  try
  {
    while (enumerator.MoveNext())
    {
      string item = enumerator.get_Current();
      Console.WriteLine(item);
    }
  }
  finally
  {
    ((global::System.IDisposable)enumerator).Dispose();
  }
}
```
