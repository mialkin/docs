# `ref` vs `out`

## ref

`ref` *requires* that the variable be initialized before it is passed.

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

* Passed variable's value can't be read by the method *until* it is set inside that method
* The method *must* set passed variable's value before returning

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

## Links

* [out parameter modifier (C# Reference) ↑](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/out-parameter-modifier)