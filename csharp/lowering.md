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

## Links

[↑ C# Lowering](https://steven-giesel.com/blogPost/69dc05d1-9c8a-4002-9d0a-faf4d2375bce).

[↑ Lowering in the C# Compiler (and what happens when you misuse it)](https://mattwarren.org/2017/05/25/Lowering-in-the-C-Compiler/).

[↑ Lowering in C# and the ability to predict code performance](https://www.youtube.com/watch?v=3oGBMGDRXVw).

[↑ https://sharplab.io — a .NET code playground](https://sharplab.io).
