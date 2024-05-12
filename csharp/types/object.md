# `object`, overriding `GetHashCode` and `Equals`, `dynamic`

## `object`

The `object` keyword is an alias for [↑ `System.Object`](https://learn.microsoft.com/en-us/dotnet/api/system.object) type, therefore `System.Object` and `object` are equivalent.

In C# type system all types, predefined and user-defined, [reference types](reference-types.md) and [value types](value-types/value-types.md), inherit directly or indirectly from `System.Object`.

An **object** is a block of memory that has been allocated and configured according to the definition of its type.

## Table of contents

- [`object`, overriding `GetHashCode` and `Equals`, `dynamic`](#object-overriding-gethashcode-and-equals-dynamic)
  - [`object`](#object)
  - [Table of contents](#table-of-contents)
    - [Methods](#methods)
      - [`Equals`](#equals)
      - [`Finalize`](#finalize)
      - [`GetHashCode`](#gethashcode)
      - [`GetType`](#gettype)
      - [`MemberwiseClone`](#memberwiseclone)
      - [`ReferenceEquals`](#referenceequals)
      - [`ToString`](#tostring)
  - [Overriding `GetHashCode` and `Equals`](#overriding-gethashcode-and-equals)
    - [Dictionary](#dictionary)
    - [Examples](#examples)
      - [Class](#class)
      - [Record](#record)
      - [Structure](#structure)
  - [`dynamic`](#dynamic)

### Methods

#### `Equals`

The [↑ `Equals`](https://learn.microsoft.com/en-us/dotnet/fundamentals/runtime-libraries/system-object-equals) method determines whether two object instances are equal.

[↑ C# difference between `==` and `Equals()`](https://stackoverflow.com/questions/814878/c-sharp-difference-between-and-equals).

#### `Finalize`

The [↑ `Finalize`](https://learn.microsoft.com/en-us/dotnet/fundamentals/runtime-libraries/system-object-finalize) method allows an object to try to free resources and perform other cleanup operations before it is reclaimed by garbage collection.

#### `GetHashCode`

The [↑ `GetHashCode`](https://learn.microsoft.com/en-us/dotnet/fundamentals/runtime-libraries/system-object-gethashcode) method provides a hash code for algorithms that need quick checks of object equality.

```csharp
public virtual int GetHashCode() => RuntimeHelpers.GetHashCode(this);
```

A hash code is a numeric value that is used to insert and identify an object in a hash-based collection, such as the `Dictionary<TKey,TValue>` class, the `Hashtable` class, or a type derived from the [↑ `DictionaryBase`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.dictionarybase) class.

Two objects that are equal return hash codes that are equal. However, the reverse is not true: equal hash codes do not imply object equality, because different (unequal) objects can have identical hash codes.

If you override the `GetHashCode` method, you should also override `Equals`, and vice versa. If your overridden `Equals` method returns `true` when two objects are tested for equality, your overridden `GetHashCode` method must return the same value for the two objects.

The `GetHashCode` method can be overridden by a derived type. If `GetHashCode` is not overridden, hash codes for reference types are computed by calling the`Object.GetHashCode` method of the base class, which computes a hash code based on an object's reference. In other words, two objects for which the [`ReferenceEquals`](#referenceequals) method returns true have identical hash codes.

If value types do not override `GetHashCode`, the [↑ `ValueType.GetHashCode`](https://learn.microsoft.com/en-us/dotnet/api/system.valuetype.gethashcode) method of the base class uses reflection to compute the hash code based on the values of the type's fields. In other words, value types whose fields have equal values have equal hash codes.

#### `GetType`

The [↑ `GetType`](https://learn.microsoft.com/en-us/dotnet/api/system.object.gettype) method gets the [↑ type declaration](https://learn.microsoft.com/en-us/dotnet/api/system.type) of the current instance.

#### `MemberwiseClone`

The [↑ `MemberwiseClone`](https://learn.microsoft.com/en-us/dotnet/api/system.object.memberwiseclone) method creates a shallow copy of the current `object`.

```csharp
protected object MemberwiseClone ();
```

Example of usage:

```csharp
A original = new A
{
    Code = 1,
    B = new B
    {
        Code = 1,
        C = new C
        {
            Code = 1
        }
    }
};

Console.WriteLine($"{original.Code}, {original.B.Code}, {original.B.C.Code}"); // 1, 1, 1

A copy = original.Clone();
Console.WriteLine($"{copy.Code}, {copy.B.Code}, {copy.B.C.Code}"); // 1, 1, 1

copy.Code = 2;
copy.B.Code = 3;
Console.WriteLine($"{original.Code}, {copy.Code}, {original.B.Code}, {copy.B.Code}"); // 1, 2, 3, 3

copy.B.C.Code = 4;
Console.WriteLine($"{original.B.C.Code}, {copy.B.C.Code}"); // 4, 4

class A
{
    public int Code { get; set; }

    public B B { get; set; }

    public A Clone()
    {
        return (A) MemberwiseClone();
    }
}

class B
{
    public int Code { get; set; }
    public C C { get; set; }
}

class C
{
    public int Code { get; set; }
}
```

#### `ReferenceEquals`

The [↑ `ReferenceEquals`](https://learn.microsoft.com/en-us/dotnet/api/system.object.referenceequals) method determines whether the specified `object` instances are the same instance.

```csharp
public static bool ReferenceEquals(object? objA, object? objB) => objA == objB;
```

Unlike the [`Equals`](#equals) method and the equality operator, the `ReferenceEquals` method cannot be overridden. Because of this, if you want to test two object references for equality and you are unsure about the implementation of the `Equals` method, you can call the `ReferenceEquals` method.

#### `ToString`

The [↑ `ToString`](https://learn.microsoft.com/en-us/dotnet/fundamentals/runtime-libraries/system-object-tostring) method returns a string that represents the current object.

```csharp
public virtual string? ToString() => this.GetType().ToString();
```

## Overriding `GetHashCode` and `Equals`

When two objects are equal they _must_ return the same hash code, but reverse is not necessary: two object may return the same hash code, but they can be not equal.

For more information about overriding GetHashCode, see the [↑ "Notes to Inheritors"](https://learn.microsoft.com/en-us/dotnet/api/system.object.gethashcode#notes-to-inheritors) section.

### Dictionary

If you override `Equals` method you must also override `GetHashCode` method, because if you don't than two objects which are in fact equal will likely end up residing in different buckets inside of `Dictionary<TKey,TValue>` of `HashSet<T>`. Therefore you will have multiple equal keys in your dictionary or hash table.

Below `GetHashCode` will be called twice: right before `a1` and `a2` are added into the dictionary; and `Equals` will be called once: before `a2` is added:

```csharp
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

### Examples

#### Class

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

#### Record

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

#### Structure

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

## `dynamic`

The [↑ `dynamic` type](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/reference-types#the-dynamic-type) is a *static* type, but an object of type `dynamic` bypasses static type checking.

C# is a [↑ statically-typed](https://en.wikipedia.org/wiki/Type_system#Static_typing), [↑ strongly-typed](https://en.wikipedia.org/wiki/Strong_and_weak_typing) language.

In most cases, an object of `dynamic` type functions like it has type `object`. The `dynamic` type differs from `object` in that operations that contain expressions of type `dynamic` aren't resolved or type checked by the compiler. The compiler packages together information about the operation, and that information is later used to evaluate the operation at run time. As part of the process, variables of type `dynamic` are compiled into variables of type `object`. Therefore, type `dynamic` exists only at compile time, not at run time.

The compiler assumes a `dynamic` element supports any operation. Therefore, you don't have to determine whether the object gets its value from a [↑ COM API](https://en.wikipedia.org/wiki/Component_Object_Model), from a dynamic language such as [↑ IronPython](https://ironpython.net), from the HTML Document Object Model, DOM, from reflection, or from somewhere else in the program. However, if the code isn't valid, errors surface at run time.

[↑ Using type dynamic](https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/interop/using-type-dynamic).
