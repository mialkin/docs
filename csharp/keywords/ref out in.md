# `ref` vs `out`

## ref

`ref` _requires_ that the variable be initialized before it is passed.

```cs
// int y; // Error below when calling A.M(ref y): Local variable 'y' might not be initialized before accessing.
int y = 1; //

A.M(ref y);
Console.WriteLine(y); // => 2

class A
{
    public static void M(ref int x)
    {
        x = 2;
    }
}
```

## out

The `out` modifier means that:

- Passed variable's value can't be read by the method _until_ it is set inside that method
- The method _must_ set passed variable's value before returning

```cs
using System;

// int y; Error below when calling Console.WriteLine(y): Local variable 'y' might not be initialized before accessing.
int y = 1;
Console.WriteLine(y); // => 1. Everything is fine.

A.M(out y);
Console.WriteLine(y); // => 2

class A
{
    public static void M(out int x)
    {
        // Console.WriteLine(x); => Compilation error: Out parameter 'x' might not be initialized before accessing.
        x = 2;
    }
}
```

Passing unassigned variable:

```cs
using System;

int y;
A.M(out y);
Console.WriteLine(y); // => 2

class A
{
    public static void M(out int x)
    {
        x = 2;
    }
}
```

Shorter version which declares an implicitly typed local variable:

```cs
using System;

A.M(out int y);
Console.WriteLine(y); // => 2

class A
{
    public static void M(out int x)
    {
        x = 2;
    }
}
```

## in

The `in` keyword complements the existing `ref` and `out` keywords to pass arguments by reference. The `in` keyword specifies passing the argument by reference, but the called method doesn't modify the value.

```csharp
int i = 10;
M(i);

void M(in int j)
{
    // Warning: The assignment target must be an assignable variable, property or indexer
    j = 5; //  Compilation error: Cannot assign to variable 'in int' because it is a readonly variable
    Console.WriteLine(j);
}
```

This addition provides a full vocabulary to express your design intent. Value types are copied when passed to a called method when you don't specify any of the following modifiers in the method signature. Each of these modifiers specifies that a variable is passed by reference, avoiding the copy. Each modifier expresses a different intent:

- `out`: This method sets the value of the argument used as this parameter.
- `ref`: This method may set the value of the argument used as this parameter.
- `in`: This method doesn't modify the value of the argument used as this parameter.

Add the `in` modifier to pass an argument by reference and declare your design intent to pass arguments by reference to avoid unnecessary copying. You don't intend to modify the object used as that argument.

This practice often improves performance for readonly value types that are larger than `IntPtr.Size`. So, apply the `in` modifier to readonly struct parameters larger than `System.IntPtr.Size` For simple types (`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal` and `bool`, and enum types), any potential performance gains are minimal. In fact, performance may degrade by using pass-by-reference for types smaller than `IntPtr.Size`.

## Links

- [↑ out parameter modifier](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/out-parameter-modifier)
- [↑ Write safe and efficient C# code](https://docs.microsoft.com/en-us/dotnet/csharp/write-safe-efficient-code)
