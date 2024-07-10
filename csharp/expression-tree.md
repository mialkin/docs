# Expression tree, `Expression`

## Table of contents

- [Expression tree, `Expression`](#expression-tree-expression)
  - [Table of contents](#table-of-contents)
  - [Expression tree](#expression-tree)
    - [Expression](#expression)
    - [Operator](#operator)
    - [Operand](#operand)
    - [Create](#create)
    - [Immutability](#immutability)
    - [Create expression tree from lambda expression](#create-expression-tree-from-lambda-expression)
  - [`Expression`](#expression-1)
  - [`Expression<TDelegate>`](#expressiontdelegate)
    - [`Expression<TDelegate>.Compile`](#expressiontdelegatecompile)

## Expression tree

An [↑ expression tree](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/expression-trees/) is a tree-like data structure in which each node represents an [expression](#expression).

### Expression

An [↑ expression](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/expressions) is a sequence of [operators](#operator) and [operands](#operand).

### Operator

The operators of an expression indicate which operations to apply to the operands.

There are three kinds of operators:

- Unary operators. The unary operators take one operand and use either prefix notation, such as `–x`, or postfix notation, such as `x++`
- Binary operators. The binary operators take two operands and all use infix notation, such as `x + y`
- Ternary operator. Only one ternary operator exists:  `?:`. It takes three operands and uses infix notation: `c ? x : y`

[↑ Infix, Prefix, and Postfix Expressions](https://www.baeldung.com/cs/infix-prefix-postfix).

Certain operators can be overloaded.

[↑ C# operators and expressions](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/)

### Operand

An **operand** is an element of an expression.

Operands can be variables, functions, pointers, and other objects:

- In the expression `2 + 3`, the numbers `2` and `3` are the operands
- In the command `x = 5`, the number `5` and the variable `x` are the operands

Operands along with operators determine the meaning and behavior of an expression.

### Create

To create expression trees by using the API, use the [`Expression`](#expression-1) class.

You can compile and run code represented by expression trees. This enables dynamic modification of executable code, the execution of LINQ queries in various databases, and the creation of dynamic queries.

The definition of "executing an expression tree" is specific to a query provider. For example, it may involve translating the expression tree to a query language appropriate for an underlying data source.

### Immutability

Expression trees are immutable. If you want to modify an expression tree, you must construct a new expression tree by copying the existing one and replacing nodes in it.

### Create expression tree from lambda expression

When a [lambda expression](/csharp/operators/lambda-expressions.md) is assigned to a variable of type `Expression<TDelegate>`, the compiler emits code to build an expression tree that represents the lambda expression.

The C# compiler can generate expression trees only from expression lambdas — single-line lambdas:

```csharp
Expression<Func<int, bool>> lambda = number => number < 5;
```

The C# compiler cannot parse statement lambdas — multi-line lambdas.

## `Expression`

The [↑ `Expression`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression) class provides the base class from which the classes that represent expression tree nodes are derived. It also contains `static` factory methods to create the various node types.

## `Expression<TDelegate>`

The [↑ `Expression<TDelegate>`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression-1) class represents a strongly typed [lambda expression](lambda-expression.md) as a data structure in the form of an [expression tree](#expression-tree).

### `Expression<TDelegate>.Compile`

An expression can be translated into a delegate, but the backward conversion is not possible.

The [↑ `Expression<TDelegate>.Compile`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression-1.compile) method compiles the lambda expression described by the expression tree into executable code and produces a delegate that represents the lambda expression.

```csharp
Expression<Func<int, bool>> expression = x => x == 1;
Func<int, bool> delegate1 = expression.Compile();
```
