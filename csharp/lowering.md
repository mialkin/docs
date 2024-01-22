# Lowering in C&#35;

A **lowering** is a transformation, done by compiler, of high-level language features into "simpler" techniques within the *same language*.

## Table of contents

- [Lowering in C#](#lowering-in-c)
  - [Table of contents](#table-of-contents)
  - [Examples](#examples)
    - [`var`](#var)
    - [`using`](#using)
    - [`foreach` array](#foreach-array)
    - [`foreach List<T>`](#foreach-listt)
    - [`i++ + ++i`](#i--i)
    - [`yield return`](#yield-return)
    - [`lock`](#lock)
  - [Links](#links)

## Examples

### `var`

```csharp
var name = "Say my name";
```

```csharp
string name = "Say my name";
```

### `using`

```csharp
using var httpClient = new HttpClient();
httpClient.GetAsync("http://example.com");
Console.WriteLine("Hello world");
```

```csharp
HttpClient httpClient = new HttpClient();
try
{
    httpClient.GetAsync("http://example.com");
    Console.WriteLine("Hello world");
}
finally
{
    if (httpClient != null)
        httpClient.Dispose();
}
```

### `foreach` array

```csharp
var list = new[] { 1, 2 };
foreach(var item in list)
    Console.WriteLine(item);
```

```csharp
int[] numArray = new int[2]{ 1, 2 };
for (int index = 0; index < numArray.Length; ++index)
    Console.WriteLine(numArray[index]);
```

### `foreach List<T>`

```csharp
var list = new List<string> { "One", "Two", "Three" };

foreach (var item in list)
    Console.WriteLine(item);
```

```csharp
List<string> stringList = new List<string>();
stringList.Add("One");
stringList.Add("Two");
stringList.Add("Three");

List<string>.Enumerator enumerator = stringList.GetEnumerator();

try
{
    while (enumerator.MoveNext())
        Console.WriteLine(enumerator.Current);
}
finally
{
    enumerator.Dispose();
}
```

### `i++ + ++i`

```csharp
var i = 2;
Console.WriteLine(i++ + ++i); // 6
```

```csharp
int num1 = 2;
int num2;
int i = num2 = num1 + 1 + 1;
Console.WriteLine(num1 + num2);
```

### `yield return`

```csharp
var provider = new NumberProvider();

foreach(var number in provider.GetNumbers())
    Console.WriteLine(number);

class NumberProvider
{
    public IEnumerable<string> GetNumbers()
    {
        yield return "One";
        yield return "Two";
    }
}
```

```csharp
IEnumerator<string> enumerator = 
    new NumberProvider().GetNumbers().GetEnumerator();

try
{
    while (enumerator.MoveNext())
        Console.WriteLine(enumerator.Current);
}
finally
{
    if (enumerator != null)
        enumerator.Dispose();
}
```

```csharp
// Decompiled with JetBrains decompiler
// Compiler-generated code is shown

using System;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics;
using System.Runtime.CompilerServices;

internal class NumberProvider
{
  [NullableContext(1)]
  [IteratorStateMachine(typeof (NumberProvider.<GetNumbers>d__0))]
  public IEnumerable<string> GetNumbers()
  {
    NumberProvider.<GetNumbers>d__0 numbers = new NumberProvider.<GetNumbers>d__0(-2);
    numbers.<>4__this = this;
    return (IEnumerable<string>) numbers;
  }

  public NumberProvider()
  {
    base..ctor();
  }

  [CompilerGenerated]
  private sealed class <GetNumbers>d__0 : 
    IEnumerable<string>,
    IEnumerable,
    IEnumerator<string>,
    IEnumerator,
    IDisposable
  {
    private int <>1__state;
    [Nullable(1)]
    private string <>2__current;
    private int <>l__initialThreadId;
    public NumberProvider <>4__this;

    [DebuggerHidden]
    public <GetNumbers>d__0(int _param1)
    {
      base..ctor();
      this.<>1__state = _param1;
      this.<>l__initialThreadId = Environment.CurrentManagedThreadId;
    }

    [DebuggerHidden]
    void IDisposable.Dispose()
    {
    }

    bool IEnumerator.MoveNext()
    {
      switch (this.<>1__state)
      {
        case 0:
          this.<>1__state = -1;
          this.<>2__current = "One";
          this.<>1__state = 1;
          return true;
        case 1:
          this.<>1__state = -1;
          this.<>2__current = "Two";
          this.<>1__state = 2;
          return true;
        case 2:
          this.<>1__state = -1;
          return false;
        default:
          return false;
      }
    }

    string IEnumerator<string>.Current
    {
      [DebuggerHidden] [return: Nullable(1)] get
      {
        return this.<>2__current;
      }
    }

    [DebuggerHidden]
    void IEnumerator.Reset()
    {
      throw new NotSupportedException();
    }

    object IEnumerator.Current
    {
      [DebuggerHidden] get
      {
        return (object) this.<>2__current;
      }
    }

    [DebuggerHidden]
    [return: Nullable(new byte[] {1, 0})]
    IEnumerator<string> IEnumerable<string>.GetEnumerator()
    {
      NumberProvider.<GetNumbers>d__0 enumerator;
      if (this.<>1__state == -2 && this.<>l__initialThreadId == Environment.CurrentManagedThreadId)
      {
        this.<>1__state = 0;
        enumerator = this;
      }
      else
      {
        enumerator = new NumberProvider.<GetNumbers>d__0(0);
        enumerator.<>4__this = this.<>4__this;
      }
      return (IEnumerator<string>) enumerator;
    }

    [DebuggerHidden]
    [return: Nullable(1)]
    IEnumerator IEnumerable.GetEnumerator()
    {
      return (IEnumerator) this.System.Collections.Generic.IEnumerable<System.String>.GetEnumerator();
    }
  }
}
```

[↑ Iterator block implementation details: auto-generated state machines](https://csharpindepth.com/Articles/IteratorBlockImplementation).

### `lock`

```csharp
var locker = new object();

lock (locker)
{
    Console.WriteLine("Hello, World!");
}
```

```csharp
object obj = new object();
bool lockTaken = false;
try
{
    Monitor.Enter(obj, ref lockTaken);
    Console.WriteLine("Hello, World!");
}
finally
{
    if (lockTaken)
      Monitor.Exit(obj);
}
```

## Links

[↑ C# Lowering](https://steven-giesel.com/blogPost/69dc05d1-9c8a-4002-9d0a-faf4d2375bce).

[↑ Lowering in the C# Compiler (and what happens when you misuse it)](https://mattwarren.org/2017/05/25/Lowering-in-the-C-Compiler/).

[↑ Lowering in C# and the ability to predict code performance](https://www.youtube.com/watch?v=3oGBMGDRXVw).

[↑ https://sharplab.io — a .NET code playground](https://sharplab.io).
