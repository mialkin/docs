# Frequently asked interview questions

## Table of contents

- [Frequently asked interview questions](#frequently-asked-interview-questions)
  - [Table of contents](#table-of-contents)
  - [LINQ](#linq)
    - [Return value of pure method is not used](#return-value-of-pure-method-is-not-used)
  - [Boxing and unboxing](#boxing-and-unboxing)
    - [Unable to cast object of type 'System.Int32' to type 'System.Int64'](#unable-to-cast-object-of-type-systemint32-to-type-systemint64)
  - [`try/catch`](#trycatch)
    - [Cannot jump out of the finally block](#cannot-jump-out-of-the-finally-block)
    - [`i++ + ++i`](#i--i)
  - [Miscellaneous](#miscellaneous)

## LINQ

### Return value of pure method is not used

```csharp
var list = new List<int>();

int i = 10;

list.Where(x => x == i).Where(x => x < 25); // Return value of pure method is not used

list.Add(10);
list.Add(15);
list.Add(20);
list.Add(25);

i = 15;

var result = list.ToList();
list.Clear();

Console.WriteLine(result.Count);
Console.WriteLine(result.FirstOrDefault());

// 4
// 10
```

## Boxing and unboxing

### Unable to cast object of type 'System.Int32' to type 'System.Int64'

```csharp
int a = 5;
object o = a;
Console.WriteLine((long)o);

// Runtime error:
// Unhandled exception. System.InvalidCastException: Unable to cast object of type 'System.Int32' to type 'System.Int64'.
//   at Program.<Main>$(String[] args) in /Users/aleksei/repositories/ConsoleApp1/src/ConsoleApp2/Program.cs:line 3
```

## `try/catch`

### Cannot jump out of the finally block

```csharp
try
{
    return 1;
}
finally
{
    return 2; // Compilation error: Cannot jump out of the finally block
}
```

### `i++ + ++i`

```csharp
var i = 2;
Console.WriteLine(i++ + ++i);
// 6
```

## Miscellaneous

- [↑ Why there is no inheritance in static classes C#?](https://stackoverflow.com/a/774225/1833895)
- `i++ + ++i`
- [↑ Why C# structs do no support inheritance](https://pragmateek.com/why-c-structs-do-no-support-inheritance/)
- [↑ How are C# Generics implemented?](https://stackoverflow.com/questions/11436802/how-are-c-sharp-generics-implemented)

```csharp
Foo foo = 1;
Console.WriteLine(foo);

record Foo(int Value)
{
    public static implicit operator Foo(int value) => new(value + 1);

    public static Foo operator *(Foo right, Foo left) => right.Value * left.Value;

    public override string ToString() => (this * 1).Value.ToString();
}
// 5
```
