# Lambda expression

A **lambda expression**, or just **lambda** for short, is an anonymous function that contains an [expression](/csharp/expression.md) or a sequence of statements.

A lambda expression with an expression on the right side of the => operator is called an expression lambda.

It's often used to create delegates or expression tree types.

Lambda expressions are particularly useful for writing LINQ query expressions.



1\. _Expression lambda_ that has an expression as its body:

```csharp
(input-parameters) => expression
```

2\. _Statement lambda_ that has a statement block as its body:

```csharp
(input-parameters) => { <sequence-of-statements> }
```

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

You can use lambda expressions in any code that requires instances of delegate types or expression trees, for example as an argument to the `Task.Run(Action)` method to pass the code that should be executed in the background.

https://learn.microsoft.com/en-us/dotnet/standard/delegates-lambdas
