# Records

Records automatically implement value equality. Consider defining a record instead of a class when your type models data and should implement value equality.

```csharp
var customer1 = new Customer("John", new DateOnly(1990, 10, 5));
var customer2 = new Customer("John", new DateOnly(1990, 10, 5));

Console.WriteLine(customer1 == customer2); // True
Console.WriteLine(customer1.Equals(customer2)); // True
Console.WriteLine(ReferenceEquals(customer1, customer2)); // False

record Customer(string Name, DateOnly BirthDate);
```

## Initialization

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

## Nondestructive mutation

A `with` expression makes a new record instance that is a copy of an existing record instance, with specified properties and fields modified.

```csharp
var customer1 = new Customer("John", new DateOnly(1990, 10, 5));
var customer2 = customer1 with { Name = "Bob" };

Console.WriteLine(customer2); // Customer { Name = Bob, BirthDate = 05.10.1990 }

record Customer(string Name, DateOnly BirthDate);
```

The result of a `with` expression is a shallow copy, which means that for a reference property, only the reference to an instance is copied. Both the original record and the copy end up with a reference to the same instance.

## Key points

While records can be mutable, they are primarily intended for supporting immutable data models. The record type offers the following features:

- Concise syntax for creating a reference type with immutable properties
- Built-in behavior useful for a data-centric reference type:
  - Value equality
  - Concise syntax for nondestructive mutation
  - Built-in formatting for display
- Support for inheritance hierarchies

You can also use structure types to design data-centric types that provide value equality and little or no behavior. But for relatively large data models, structure types have some disadvantages:

- They don't support inheritance
- They're less efficient at determining value equality. For value types, the `ValueType.Equals` method uses reflection to find all fields. For records, the compiler generates the `Equals` method. In practice, the implementation of value equality in records is measurably faster
- They use more memory in some scenarios, since every instance has a complete copy of all of the data. Record types are reference types, so a record instance contains only a reference to the data

## Links

[↑ Records (C# reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/record#nondestructive-mutation).
