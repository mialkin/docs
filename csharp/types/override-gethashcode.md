# Override `GetHashCode` when `Equals` is overridden

When two objects are equal they _must_ return the same hash code, but reverse is not necessary: two object may return the same hash code, but they can be not equal.

 For more information about overriding GetHashCode, see the [↑ "Notes to Inheritors"](https://learn.microsoft.com/en-us/dotnet/api/system.object.gethashcode#notes-to-inheritors) section.

## Table of contents

- [Override `GetHashCode` when `Equals` is overridden](#override-gethashcode-when-equals-is-overridden)
  - [Table of contents](#table-of-contents)
  - [Dictionary](#dictionary)
  - [Examples](#examples)
    - [Class](#class)
    - [Record](#record)
    - [Structure](#structure)
  - [Links](#links)

## Dictionary

If you override `Equals` method you must also override `GetHashCode` method, because if you don't than two objects which are in fact equal will likely end up residing in different buckets inside of `Dictionary<TKey,TValue>` of `HashSet<T>`. Therefore you will have multiple equal keys in your dictionary or hash table.

Below `GetHashCode` will be called twice: right before `a1` and `a2` are added into the dictionary; and `Equals` will be called once: before `a2` is added:

```csharp
using System;
using System.Collections.Generic;

A a1 = new();
A a2 = new();

var dict = new Dictionary<A, string>();

dict.Add(a1, "one");
dict.Add(a2, "two");

Console.WriteLine("End");

class A
{

    public override bool Equals(object? obj)
    {
        return false;
    }

    public override int GetHashCode()
    {
        return 12345;
    }
}
```

Before adding a new item to the dictionary the `GetHashCode` method of the item's key object is called to determine bucket's index of the hash table that backs up the dictionary. `Equals` is called on the item's key object only if there is an item already in the bucket that corresponds to the `int` returned by `GetHashCode`, and it's called as many times as many items are already in that bucket.

Now `Equals` returns always `true`. If you would make it to always return `false`, then the code above will throw "_An item with the same key has already been added_" exception. One way to avoid this exception would be to use `dict[a2] = "two"` instead of `dict.Add(a2, "two")`, the former will overwrite existing value if key already exists in the dictionary.

Now if you will try to run this code:

```csharp
string str1 = dict[a1];
```

first it will call `GetHashCode` on `a1` key to figure out bucket's index. Then it will call `Equals` on `a1`'s and `a2`'s keys that are already in the bucket. Since `Equals` returns always `false`, then it will raise "The given key 'A' was not present in the dictionary" exception.

Also take a look at [↑ `HashCode.Combine`](https://docs.microsoft.com/en-us/dotnet/api/system.hashcode.combine) method that combines several values into a single hash code.

## Examples

### Class

```csharp
var user1 = new User("Bob", new DateOnly(1990, 10, 5));
var user2 = new User("Bob", new DateOnly(2000, 10, 5));
var user3 = new User("Bob", new DateOnly(1990, 10, 5));

Console.WriteLine(user1.GetHashCode()); // 2007968221
Console.WriteLine(user2.GetHashCode()); // -2132409190
Console.WriteLine(user3.GetHashCode()); // 2007968221

Console.WriteLine(user1 == user3); // True
Console.WriteLine(Equals(user1, user3)); // True
Console.WriteLine(ReferenceEquals(user1, user3)); // False

class User
{
    public User(string name, DateOnly dateOfBirth)
    {
        Name = name;
        DateOfBirth = dateOfBirth;
    }

    public string Name { get; init; }
    public DateOnly DateOfBirth { get; init; }

    public override bool Equals(object? obj)
    {
        return obj is User other && Equals(other);
    }

    private bool Equals(User other)
    {
        return Name == other.Name && DateOfBirth.Equals(other.DateOfBirth);
    }

    public override int GetHashCode()
    {
        return HashCode.Combine(Name, DateOfBirth);
    }
    
    public static bool operator ==(User left, User right)
    {
        return left.Equals(right);
    }

    public static bool operator !=(User left, User right)
    {
        return !left.Equals(right);
    }
}
```

### Record

```csharp
var customer1 = new Customer("John", new DateOnly(1990, 10, 5));
var customer2 = new Customer("John", new DateOnly(1990, 10, 5));

Console.WriteLine(customer1.GetHashCode()); // 802341848
Console.WriteLine(customer2.GetHashCode()); // 802341848

Console.WriteLine(customer1 == customer2); // True
Console.WriteLine(customer1.Equals(customer2)); // True
Console.WriteLine(ReferenceEquals(customer1, customer2)); // False

record Customer(string Name, DateOnly BirthDate);
```

### Structure

```csharp
var user1 = new User("Bob", new DateOnly(1990, 10, 5));
var user2 = new User("Bob", new DateOnly(2000, 10, 5));
var user3 = new User("Bob", new DateOnly(1990, 10, 5));

Console.WriteLine(user1.GetHashCode()); // 1075247716
Console.WriteLine(user2.GetHashCode()); // 1075247716
Console.WriteLine(user3.GetHashCode()); // 1075247716

Console.WriteLine(user1 == user3); // True
Console.WriteLine(Equals(user1, user3)); // True
Console.WriteLine(ReferenceEquals(user1, user3)); // False. Warning: 'Object.ReferenceEquals' is always false because it is called with value type

struct User
{
    public User(string name, DateOnly dateOfBirth)
    {
        Name = name;
        DateOfBirth = dateOfBirth;
    }

    public string Name { get; init; }
    public DateOnly DateOfBirth { get; init; }

    public static bool operator ==(User left, User right)
    {
        return left.Equals(right);
    }

    public static bool operator !=(User left, User right)
    {
        return !left.Equals(right);
    }
}
```

## Links

- [↑ Equality Operators](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/equality-operators)
- [↑ How to define value equality for a class or struct](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type)
