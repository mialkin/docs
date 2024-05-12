# `class`, `record`

## Table of contents

- [`class`, `record`](#class-record)
  - [Table of contents](#table-of-contents)
  - [`class`](#class)
  - [`record`](#record)
    - [Initialization](#initialization)
    - [Non-destructive mutation](#non-destructive-mutation)

## `class`

The `class` is a keyword that is used to declare *classes*.

Class types support *inheritance* — mechanism whereby a derived class can extend and specialize a base class.

A **class** is a data structure that may contain:

1. Data members: constants and fields
2. Function members: methods, properties, events, indexers, operators, instance constructors, finalizers, and static constructors
3. Nested types

## `record`

The [↑ `record`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/record) modifier to define a reference type that provides built-in functionality for encapsulating data.

C# 10 allows the `record class` syntax as a synonym to clarify a [reference type](reference-types.md), and `record struct` to define a [value type](value-types/value-types.md) with similar functionality.

Records automatically implement value equality.

Consider defining a record instead of a class when your type models data and should implement value equality.

```csharp
var customer1 = new Customer("John", new DateOnly(1990, 10, 5));
var customer2 = new Customer("John", new DateOnly(1990, 10, 5));

Console.WriteLine(customer1 == customer2); // True
Console.WriteLine(customer1.Equals(customer2)); // True
Console.WriteLine(ReferenceEquals(customer1, customer2)); // False

record Customer(string Name, DateOnly BirthDate);
```

### Initialization

You can create `record` types with immutable properties by using positional parameters:

```csharp
var person = new Person("John", "Smith");
// person.LastName = "Joneses"; // Not going to work: Init-only property 'Person.LastName' can only be assigned in an
// object initializer, or on 'this' or 'base' in an instance constructor or an 'init' accessor 

public record Person(string FirstName, string LastName);
```

or standard property syntax:

```csharp
var person = new Person
{
    FirstName = "John",
    LastName = "Smith"

};

// person.LastName = "Joneses"; // Init-only property 'Person.LastName' can only be assigned in an object initializer, or on 'this' or 'base' in an instance constructor or an 'init' accessor

public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
};
```

You can also create record types with mutable properties and fields:

```csharp
var person = new Person
{
    FirstName = "John",
    LastName = "Smith"

};

person.LastName = "Joneses";

Console.WriteLine(person); // Person { FirstName = John, LastName = Joneses }
// If record keyword is replaced with class keyword than Console.WriteLine would print just "Person"

public record Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
};
```

### Non-destructive mutation

A `with` expression makes a new record instance that is a copy of an existing record instance, with specified properties and fields modified.

```csharp
var customer1 = new Customer("John", new DateOnly(1990, 10, 5));
var customer2 = customer1 with { Name = "Bob" };

Console.WriteLine(customer2); // Customer { Name = Bob, BirthDate = 05.10.1990 }

record Customer(string Name, DateOnly BirthDate);
```

The result of a `with` expression is a shallow copy, which means that for a reference property, only the reference to an instance is copied. Both the original record and the copy end up with a reference to the same instance.
