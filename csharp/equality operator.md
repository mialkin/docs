# Equality operator ==

## Reference types equality

By default, two reference-type operands are equal if they refer to the same object.

```csharp
using System;

class Program
{
    public static void Main()
    {
        A a = new A(5);
        A a2 = new A(5);

        Console.WriteLine(a == a2);
        a2 = a;
        Console.WriteLine(a == a2);
    }
}

class A
{
    private readonly int _i;

    public A(int i)
    {
        _i = i;
    }
}
```

Output:

```output
False
True
```

Two objects are equal when both of them are `null`:

```csharp

using System;

class Program
{
    public static void Main()
    {
        object o = null;
        object o2 = null;

        Console.WriteLine(o == o2);
    }
}
```

Output:

```output
True
```

## String equality

```csharp
using System;

class Program
{
    public static void Main()
    {
        string str = "spring";
        string str2 = "spring";

        Console.WriteLine(str == str2);

        str = null;
        str2 = null;

        Console.WriteLine(str == str2);
    }
}
```

Output:

```output
True
True
```

[↑ Equality operators](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/equality-operators)
