# Lambda expressions

A lambda expression is an expression of any of the following two forms:

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
