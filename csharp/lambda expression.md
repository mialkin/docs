# Lambda expression

A lambda expression is an expression of any of the following two forms:

1\. _Expression lambda_ that has an expression as its body:

```csharp
(input-parameters) => expression
```

2\. _Statement lambda_ that has a statement block as its body:

```csharp
(input-parameters) => { <sequence-of-statements> }
```

Any lambda expression can be converted to a [delegate](delegate.md) type. The delegate type to which a lambda expression can be converted is defined by the types of its parameters and return value. If a lambda expression doesn't return a value, it can be converted to one of the `Action` delegate types; otherwise, it can be converted to one of the `Func` delegate types.

You can use lambda expressions in any code that requires instances of delegate types or [expression trees](expression%20tree.md), for example as an argument to the `Task.Run(Action)` method to pass the code that should be executed in the background.

## Links

[↑ Lambda expressions (C# reference)](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions)
