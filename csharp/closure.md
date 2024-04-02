# Closure

A **closure** is a [first-class function](#first-class-function) that captures [free variables](#free-variable) from its surrounding environment.

## Table of contents

- [Closure](#closure)
  - [Table of contents](#table-of-contents)
  - [Examples](#examples)
  - [First-class function](#first-class-function)
  - [Free variable](#free-variable)
  - [Links](#links)

## Examples

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

<https://medium.com/swlh/the-magic-of-c-closures-9c6e3fff6ff9>

<https://www.linkedin.com/pulse/c-interview-questions-you-might-get-what-closure-artem-naruzhnii/>

<https://chat.openai.com/c/3cec0a36-f0b4-424a-9c8c-709b19a84be4>

[↑ How do closures work behind the scenes? (C#)](https://stackoverflow.com/questions/1928636/how-do-closures-work-behind-the-scenes-c).
