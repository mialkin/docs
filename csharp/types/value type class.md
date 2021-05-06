# `ValueType` class

Provides the base class for value types.

```csharp
public abstract class ValueType
```

`ValueType` overrides the virtual methods from `Object` with more appropriate implementations for value types.

Data types are separated into value types and reference types. Value types are either stack-allocated or allocated inline in a structure. Reference types are heap-allocated. Both reference and value types are derived from the ultimate base class `Object`. In cases where it is necessary for a value type to behave like an object, a wrapper that makes the value type look like a reference object is allocated on the heap, and the value type's value is copied into it. The wrapper is marked so the system knows that it contains a value type. This process is known as boxing, and the reverse process is known as unboxing. Boxing and unboxing allow any type to be treated as an object.

Although `ValueType` is the implicit base class for value types, you cannot create a class that inherits from `ValueType` directly. Instead, individual compilers provide a language keyword or construct (such as `struct` in C#) to support the creation of value types.

Aside from serving as the base class for value types in the .NET Framework, the `ValueType` structure is generally not used directly in code. However, it can be used as a parameter in method calls to restrict possible arguments to value types instead of all objects, or to permit a method to handle a number of different value types:

```csharp
Point p = new Point(1, 1);
M(p);   // Point

int i = 1;
M(i);   // 1

string str = "Hello";
M("Hello");     // Compile-time error: Argument type 'string' is not assignable to parameter type 'System.ValueType'.

void M(ValueType vt)
{
    Console.WriteLine(vt);
}

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
