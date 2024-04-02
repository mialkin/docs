# Closure

A **closure** is a [first-class function](#first-class-function) that captures [free variables](#free-variable) from its surrounding scope.

Operationally, a closure is a record storing a function together with an environment : a mapping associating each free variable of the function (variables that are used locally, but defined in an enclosing scope) with the value or storage location to which the name was bound when the closure was created. A closure — unlike a plain function — allows the function to access those captured variables through the closure's reference to them, even when the function is invoked outside their scope.

## Table of contents

- [Closure](#closure)
  - [Table of contents](#table-of-contents)
  - [Examples](#examples)
  - [How does it work?](#how-does-it-work)
  - [First-class function](#first-class-function)
  - [Free variable](#free-variable)
  - [Links](#links)

## Examples

1\)

```csharp
var addFiveAndIncrement = AddFiveAndIncrement();

Console.WriteLine(addFiveAndIncrement(10));
Console.WriteLine(addFiveAndIncrement(10));
Console.WriteLine(addFiveAndIncrement(10));

Func<int, int> AddFiveAndIncrement()
{
    var delta = 5;

    int Local(int input)
    {
        delta++;
        return input + delta;
    }
    
    return Local;
}

// Output:
// 16
// 17
// 18
```

2\)

Closures capture the *variable* and not the *value*, that's why we get following result:

```csharp
var actions = new Action[10];

for (int i = 0; i < 10; i++)
{
    // Captured variable `i` is modified in the outer scope.
    actions[i] = () => Console.Write($"{i} ");
}

foreach (var action in actions)
{
    action();
}

// Output:
// 10 10 10 10 10 10 10 10 10 10 
```

To get the code to do as we want, we can change our loop block to declare a variable inside the loop block:

```csharp
var actions = new Action[10];

for (int i = 0; i < 10; i++)
{
    var j = i;
    actions[i] = () => Console.Write($"{j} ");
}

foreach (var action in actions)
{
    action();
}

// Output:
// 0 1 2 3 4 5 6 7 8 9 
```

## How does it work?

C# compiler detects when a delegate forms a closure which is passed out of the current scope and it promotes the delegate, and the associated local variables into a compiler generated class. 

This way, it simply needs a bit of compiler trickery to pass around an instance of the compiler generated class, so each time we invoke the delegate we are actually calling the method on this class. Once we are no longer holding a reference to this delegate, the class can be garbage collected and it all works exactly as it is supposed to!

## First-class function

A **first-class function** is a function which programming language treats as a first-class data type. It means that you can assign a function to a variable, pass it around, and invoke it.

In C# we can create a first-class function using:

1\) anonymous delegate:

```csharp
var addFive = delegate(int input)
{
    return input + 5;
};
```

2\) lambda expression:

```csharp
Func<int, int> addFive = input =>
{
    return input + 5;
};
```

3\) local function:

```csharp
var addFive = AddFive();

Func<int, int> AddFive()
{
    return Local;

    int Local(int input)
    {
        return input + 5;
    }
}
```

## Free variable

A **free variable** is a variable referenced in a function which is not a parameter of the function or a local variable of the function. It might look like this:

```csharp
var five = 5;

Func<int, int> addFive = input =>
{
    return input + five;
};
```

## Links

[↑ A Simple Explanation of C# Closures](https://www.simplethread.com/c-closures-explained).

[↑ The magic of C# Closures](https://medium.com/swlh/the-magic-of-c-closures-9c6e3fff6ff9).

<https://www.linkedin.com/pulse/c-interview-questions-you-might-get-what-closure-artem-naruzhnii/>

<https://chat.openai.com/c/3cec0a36-f0b4-424a-9c8c-709b19a84be4>

[↑ How do closures work behind the scenes? (C#)](https://stackoverflow.com/questions/1928636/how-do-closures-work-behind-the-scenes-c).
