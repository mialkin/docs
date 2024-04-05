# Lambda expression

A **lambda expression**, or just **lambda** for short, is an anonymous function that contains an [expression](/csharp/expression.md) or a sequence of [statements](/csharp/statement.md).

The [↑ lambda declaration operator `=>`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-operator) is used to separate the lambda's parameter list from its body.

An **expression lambda** is a lambda expression that has an [expression](/csharp/expression.md) as its body:

```csharp
(input parameters) => expression
```

A **statement lambda** is a a lambda expression that has a [statement](/csharp/statement.md) block as its body:

```csharp
(input parameters) => { sequence of statements }
```

## Usage

Lambda expressions often used to create [delegates](/csharp/types/delegate.md) or [expression tree](/csharp/expression-tree.md) types.

Any lambda expression can be converted to a delegate type:

```csharp
M(x => x * 3);

void M(Whatever whatever)
{
    Console.WriteLine(whatever.Invoke(5));
}

delegate int Whatever(int value);
```

Output:

```output
15
```

Lambda expressions are particularly useful for writing LINQ query expressions.

You can use lambda expressions in any code that requires instances of delegate types or expression trees, for example as an argument to the `Task.Run(Action)` method to pass the code that should be executed in the background.

https://learn.microsoft.com/en-us/dotnet/standard/delegates-lambdas
