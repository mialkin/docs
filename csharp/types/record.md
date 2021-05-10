# Records

You can create `record` types with immutable properties by using positional parameters:

```csharp
Person person = new("John", "Smith");

public record Person(string FirstName, string LastName);
```

or standard property syntax:

```csharp
public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
};
```

You can also create record types with mutable properties and fields:

```csharp
public record Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
};
```

While records can be mutable, they are primarily intended for supporting immutable data models. The record type offers the following features:

- Concise syntax for creating a reference type with immutable properties
- Built-in behavior useful for a data-centric reference type:
  - Value equality
  - Concise syntax for nondestructive mutation
  - Built-in formatting for display
- Support for inheritance hierarchies

You can also use structure types to design data-centric types that provide value equality and little or no behavior. But for relatively large data models, structure types have some disadvantages:

- They don't support inheritance.
- They're less efficient at determining value equality. For value types, the `ValueType.Equals` method uses reflection to find all fields. For records, the compiler generates the `Equals` method. In practice, the implementation of value equality in records is measurably faster.
- They use more memory in some scenarios, since every instance has a complete copy of all of the data. Record types are reference types, so a record instance contains only a reference to the data.