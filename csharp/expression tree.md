# Expression tree

Expression trees represent code in a tree-like data structure, where each node is an expression, for example, a method call or a binary operation such as x < y.

You can compile and run code represented by expression trees. This enables dynamic modification of executable code, the execution of LINQ queries in various databases, and the creation of dynamic queries.

## Creating expression trees from lambda expressions

When a lambda expression is assigned to a variable of type `Expression<TDelegate>`, the compiler emits code to build an expression tree that represents the lambda expression.

The C# compiler can generate expression trees only from expression lambdas (or single-line lambdas). It cannot parse statement lambdas (or multi-line lambdas).

```csharp
Expression<Func<int, bool>> lambda = num => num < 5;
```

## Creating expression trees by using the API

To create expression trees by using the API, use the `Expression` class. This class contains static factory methods that create expression tree nodes of specific types.

## Immutability of Expression Trees

Expression trees should be immutable. This means that if you want to modify an expression tree, you must construct a new expression tree by copying the existing one and replacing nodes in it.

## Compiling expression trees

The Expression<TDelegate> type provides the Compile method that compiles the code represented by an expression tree into an executable delegate.

## Links

[↑ Expression Trees (C#)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/expression-trees/)
