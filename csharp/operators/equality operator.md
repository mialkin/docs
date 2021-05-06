# Equality operator ==

- [Equality operator ==](#equality-operator-)
  - [Reference types equality](#reference-types-equality)
  - [String equality](#string-equality)
  - [`struct` type equality](#struct-type-equality)
  - [Links](#links)

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

## `struct` type equality

This code will not compile because user-defined `struct` types don't support the `==` operator by default:

```csharp
Point p1 = new Point(1, 1);
Point p2 = new Point(1, 1);

Console.WriteLine(p1 == p2); // Compile-time error: Cannot apply operator '==' to operands of type 'Point' and 'Point'

struct Point
{
    public int X;
    public int Y;

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }
}
```

For structs, the default implementation of `Object.Equals(Object)` (which is the overridden version in `System.ValueType`) performs a value equality check by using reflection to compare the values of every field in the type:

```csharp
Point p1 = new Point(1, 1);
Point p2 = new Point(1, 1);
Point p3 = new Point(1, 2);

Console.WriteLine(p1.Equals(p2));   // True
Console.WriteLine(p1.Equals(p3));   // False

struct Point
{
    public int X;
    public int Y;

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }
}
```

When an implementer overrides the virtual `Equals` method in a struct, the purpose is to provide a more efficient means of performing the value equality check and optionally to base the comparison on some subset of the struct's field or properties.

To support the `==` operator, a user-defined `struct` must overload it:

```csharp
Point p1 = new Point(1, 1);
Point p2 = new Point(1, 1);

Console.WriteLine(p1 == p2);        // True
Console.WriteLine(p1.Equals(p2));   // True

struct Point
{
    public int X;
    public int Y;

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public static bool operator ==(Point lhs, Point rhs)
    {
        return lhs.X == rhs.X && lhs.Y == rhs.Y;
    }

    public static bool operator !=(Point lhs, Point rhs)
    {
        return !(lhs == rhs);
    }
}
```

## Links

- [Equality operators ↑](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/equality-operators)
- [How to define value equality for a class or struct ↑](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type)
