# Functional programming

The **functional programming** is programming with *mathematical functions*.

[↑ Functional Extensions for C#](https://github.com/vkhorikov/CSharpFunctionalExtensions).

## Table of contents

- [Functional programming](#functional-programming)
  - [Table of contents](#table-of-contents)
  - [Mathematical function](#mathematical-function)
  - [Why](#why)
  - [Vocabulary](#vocabulary)
    - [Immutability](#immutability)
    - [State](#state)
    - [Side effect](#side-effect)
  - [Importance of immutability](#importance-of-immutability)
  - [Outline](#outline)

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

## Vocabulary

### Immutability

The term **immutability** applied to data structure, such as a class, means that the object of this class cannot change during their lifetime.

There are several types of immutability with their own nuances, but they are not essential for us right now. For the most part we can say that a class is either mutable, meaning that its instances can change in some way or another, or immutable, meaning that once we create an instance of that class we cannot modify it later on.

### State

A **state** represents data that changes over time.

It's important to understand that state is not just data that compromises a class, state is a subset of this data that changes during its lifetime. An immutable class doesn't have any state in that sense, only mutable classes do.

### Side effect

A **side effect** is a change that is made to some state.

An operation leaves a side effect if it mutates an instance of a class, updates a file on the disk or saves some data to the database.

## Importance of immutability

Mutable operations make your code dishonest. The signature off such a method no longer tells us what the actual result of the operation is and that hinders our ability to reason about the code. In order to validate your assumptions about the code you are writing, not only do you have to look at the methods signature itself, but you also need to fall down to the implementation details and see if this method leaves some side effect that you didn't expect.

Overall code with data structures that change over time is harder to debug and is more error prone. It brings even more problems in multi-threaded application where you can have all sorts of race conditions.

On the other hand, when you operate immutable data only, you force yourself to reveal hidden side effects by stating them in the method signature and thus making it honest.

The practice of using immutable data makes the code more readable because we don't have to fall down to the method's implementation details in order to reason about the program flow.

Aside from code readability, this approach has other merits as well. First of all, with immutable classes we need to validate the *invariants* only once in the constructor. Once we've created an instance of an immutable class we can be absolutely sure it resides in a valid state. Also immutability automatically makes the code thread safe. So we don't have to worry about synchronization and race conditions.

An **invariant** is a condition that must be held true at all times. For example, a triangle is a concept that has 3 edges. The `edges.Count == 3` condition is inherently true for all triangles.

[↑ Validation vs Invariants](https://khorikov.org/posts/2022-06-06-validation-vs-invariants/).

## Outline

- Immutable architecture
- Why `null`s are evil and how to fix that with `Maybe` type
- Primitive obsession
- The use of exceptions
- Handling failure & input errors
