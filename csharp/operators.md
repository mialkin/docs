# Equality operator `==`, `stackalloc`

## Table of contents

- [Equality operator `==`, `stackalloc`](#equality-operator--stackalloc)
  - [Table of contents](#table-of-contents)
  - [Equality operator `==`](#equality-operator-)
    - [Value types equality](#value-types-equality)
    - [Reference types equality](#reference-types-equality)
    - [Record types equality](#record-types-equality)
      - [String equality](#string-equality)
      - [Structures equality](#structures-equality)
  - [`stackalloc`](#stackalloc)

## Equality operator `==`

The [↑ equality operator `==`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/equality-operators#equality-operator-) returns `true` if its operands are equal, `false` otherwise.

### Value types equality

Operands of the built-in [value types](/csharp/types/value-types/value-types.md) are equal if their values are equal.

### Reference types equality

By default, two [reference type](/csharp/types/reference-types/reference-types.md) operands are equal if they refer to the same object of if both of them are `null`:

If a reference type overloads the `==` operator, use the `Object.ReferenceEquals` method to check if two references of that type refer to the same object.

### Record types equality

Record types support the `==` and `!=` operators that by default provide value equality semantics. That is, two record operands are equal when both of them are `null` or corresponding values of all fields and auto-implemented properties are equal.

#### String equality

Two string operands are equal when both of them are `null` or both string instances are of the same length and have identical characters in each character position:

```csharp
var s1 = "hello!";
var s2 = "HeLLo!";
Console.WriteLine(s1 == s2.ToLower()); // True

Console.WriteLine(s1 == s2); // False

string str3 = null;
string str4 = null;

Console.WriteLine(str3 == str4); // True
```

#### Structures equality

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

For structs, the default implementation of `Object.Equals(Object)`, which is the overridden version in `System.ValueType`, performs a value equality check by using reflection to compare the values of every field in the type:

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

- [↑ How to define value equality for a class or struct](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type)

## `stackalloc`

A [↑ `stackalloc`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/stackalloc) expression allocates a block of memory on the stack. A stack-allocated memory block created during the method execution is automatically discarded when that method returns. You can't explicitly free the memory allocated with `stackalloc`. A stack allocated memory block isn't subject to garbage collection and doesn't have to be pinned with a [`fixed`](/csharp/memory/garbage-collector.md#fixed-keyword) statement.

You can assign the result of a `stackalloc` expression to a variable of one of the following types: [`Span<T>`, `ReadOnlySpan<T>`](/csharp/types/value-types/struct-types.md#spant-readonlyspant), [↑ pointer type](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/unsafe-code#pointer-types).

You don't have to use an [↑ unsafe](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/unsafe) context when you assign a stack allocated memory block to a `Span<T>` or `ReadOnlySpan<T>` variable:

```csharp
int length = 3;
Span<int> numbers = stackalloc int[length];
for (var i = 0; i < length; i++)
{
    numbers[i] = i;
}
```

You must use an `unsafe` context when you work with pointer types:

```csharp
unsafe
{
    int length = 3;
    int* numbers = stackalloc int[length];
    for (var i = 0; i < length; i++)
    {
        numbers[i] = i;
    }
}
```
