# Statement

A **statement** is a complete unit of execution that performs some action.

Statements can perform various actions such as declaring variables, assigning values, controlling the flow of execution, and calling methods.

## Table of contents

- [Statement](#statement)
  - [Table of contents](#table-of-contents)
  - [Common types of statements](#common-types-of-statements)
    - [Declaration statements](#declaration-statements)
    - [Assignment statements](#assignment-statements)
    - [Expression statements](#expression-statements)
    - [Selection statements](#selection-statements)
    - [Iteration statements](#iteration-statements)
    - [Jump statements](#jump-statements)
    - [Exception handling statements](#exception-handling-statements)
  - [Links](#links)

## Common types of statements

### Declaration statements

Declaration statements declare variables or constants.

```csharp
int x;
const double PI = 3.14;
```

### Assignment statements

Assignment statements assign values to variables.

```csharp
x = 10;
```

### Expression statements

Expression Statements consist of expressions that are evaluated.

```csharp
x = x + 5;
```

### Selection statements

Selection statements control the flow of execution based on conditions. Examples include `if`, `else if`, `else`, and `switch` statements.

```csharp
if (x > 0)
{
    // Do something.
}
else
{
    // Do something else.
}
```

### Iteration statements

Iteration statements execute a block of code repeatedly. Examples include `for`, `foreach`, `while`, and `do-while` statements.

```csharp
for (int i = 0; i < 10; i++)
{
    // Do something repeatedly.
}
```

### Jump statements

Jump statements alter the flow of control by transferring execution to another part of the program. Examples include `break`, `continue`, `return`, and `goto` statements.

```csharp
return x;
```

### Exception handling statements

Exception handling statements handle exceptions that occur during program execution. Examples include `try`, `catch`, `finally`, and `throw` statements.

```csharp
try
{
    // Code that might throw an exception.
}
catch (Exception ex)
{
    // Handle the exception.
}
```

## Links

[↑ Statements](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/statements).
