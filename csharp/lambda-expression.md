# Lambda expression, anonymous function

A **lambda expression**, or just **lambda** for short, is an [anonymous function](#anonymous-function) that contains an [expression](/csharp/expression-tree.md#expression) or a sequence of [statements](/csharp/statement.md).

The [↑ lambda declaration operator `=>`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-operator) is used to separate the lambda's parameter list from its body.

An **expression lambda** is a lambda expression that has an [expression](/csharp/expression-tree.md#expression) as its body:

```csharp
(input parameters) => expression
```

A **statement lambda** is a a lambda expression that has a [statement](/csharp/statement.md) block as its body:

```csharp
(input parameters) => { sequence of statements }
```

## Anonymous function

An **anonymous function** is a function that is defined without a name.

Anonymous function is typically used when you need to pass a small piece of logic as an argument to another function, such as in LINQ queries, event handling, or asynchronous programming. Anonymous functions are useful when you don't want to create a separate named method for a simple operation.

The `delegate` operator creates an anonymous function that can be converted to a delegate type:

```csharp
Func<int, int, int> sum = delegate (int a, int b) { return a + b; }
```

Lambda expressions provide a more concise and expressive way to create an anonymous function:

```csharp
Func<int, int, int> sum = (a, b) => a + b;
```

## Relation to `delegate` and `Expression` types

Lambda expressions can be compiled to [delegate](/csharp/types/delegate.md) and to [expression tree](/csharp/expression-tree.md) types:

```csharp
SumDelegate((x, y) => x + y, 6, 8);
SumExpression((x, y) => x + y, 5, 7);

void SumDelegate(Func<int, int, int> function, int a, int b)
{
    Console.WriteLine("SumDelegate: " + function(a, b));
}

void SumExpression(Expression<Func<int, int, int>> expression, int a, int b)
{
    Console.WriteLine("SumExpression: " + expression.Compile().Invoke(a, b));
}

// Output:
// SumDelegate: 14
// SumExpression: 12
```
