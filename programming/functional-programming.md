# Functional programming

The **functional programming** is programming with *mathematical functions*.

## Mathematical function

The best way to think of a mathematical function is as of a pipe that transforms any value we pass into it to another value. A mathematical function doesn't leave any marks in the outside world about its existence.

Mathematical function must meet the following two requirements:

1. **Referential transparency**: whenever you supply the same arguments it always returns the same result
2. **Method signature honesty**: it's signature must convey all information about the possible input failures it accepts and possible outcome it produces

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

## Why

One of the biggest problems that come into play in enterprise software development is complexity. Complexity affects such things as speed of development, the number of bugs and the ability to quickly adjust to ever changing market needs.

There is only so much of complexity we can deal with at a time. If the project's code base overwhelms this limit, it becomes really hard and at some point even impossible to change anything in the software without introducing some unexpected side effects, and that slows the development down and may eventually lead to a project failure.

Applying functional programming principles help reduce code complexity and thus prevent a disaster because of the two traits they possess: method signature honesty and referential transparency, we are able to understand and reason about the code much easier.

Every method in our code base, if written as a mathematical function, can be considered in isolation from others. When we are sure that our methods don't affect the global state or blow up with an exception, we can treat them as building blocks and compose in any way we want. This in turn unlocks great opportunities for building complex functionality, which itself is not much harder to create than the parts it consists of.

With honest method signatures we don't have to fall down to the methods implementation or refer to the documentation to see if there is something else we need to consider before using it. The signature itself tells us what can happen after we call it.

Unit testing of such code also becomes much easier. It comes down to a couple of lines where you need to provide an input failure and check that the result is the one you expected. No need to create complex text doubles, such as mocks, and maintain them later on.

## Outline

- Immutable architecture
- Why `null`s are evil and how to fix that with `Maybe` type
- Primitive obsession
- The use of exceptions
- Handling failure & input errors
