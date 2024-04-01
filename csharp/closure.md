# Closure

A **closure** is a *first-class function* that captures *free variables* from its surrounding environment.

A **first-class function** is a function which programming language treats as a first-class data type. It means that you can assign a function to a variable, pass it around, and invoke it.

In C# we can create a first-class function using anonymous delegate:

```csharp
var addFive = delegate(int input)
{
    return input + 5;
};
```

or using lambda expression:

```csharp
Func<int, int> addFive = input =>
{
    return input + 5;
};
```

A **free variable** is a variable referenced in a function which is not a parameter of the function or a local variable of the function. It might look like this:

```csharp
var five = 5;

Func<int, int> addFive = input =>
{
    return input + five;
};
```

<https://www.simplethread.com/c-closures-explained/>

<https://medium.com/swlh/the-magic-of-c-closures-9c6e3fff6ff9>

<https://www.linkedin.com/pulse/c-interview-questions-you-might-get-what-closure-artem-naruzhnii/>

<https://chat.openai.com/c/3cec0a36-f0b4-424a-9c8c-709b19a84be4>

[↑ How do closures work behind the scenes? (C#)](https://stackoverflow.com/questions/1928636/how-do-closures-work-behind-the-scenes-c).
