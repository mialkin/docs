# Functional programming

The **functional programming** is programming with *mathematical functions*.

## Mathematical function

The best way to think of a mathematical function is as of a pipe that transforms any value we pass into it to another value. A mathematical function doesn't leave any marks in the outside world about its existence.

Mathematical function must meet the following two requirements:

1. **Referential transparency**: whenever you supply the same arguments it always returns the same result
2. **Attribute method signature honesty**: it's signature must convey all information about the possible input failures it accepts and possible outcome it produces

This is not a mathematical function:

```csharp
public static int Divide(int x, int y) {
    return x / y;
}
```

It's because if we pass `0` as second argument we'll get `DivideByZeroException` instead of `int` value as function's signature states.

This is a mathematical function:

```csharp
public static int Divide(int x, NonZeroInteger y) {
    return x / y.Value;
}
```

This is a mathematical function:

```csharp
public static int? Divide(int x, int y){
    if (y == 0)
        return null;

    return x / y;    
}
```

- Immutable architecture
- Why `null`s are evil and how to fix that with `Maybe` type
- Primitive obsession
- The use of exceptions
- Handling failure & input errors

