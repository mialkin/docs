# The Liskov Subsititution Principles (LSP)

The principle states:

> Subclasses should be substituted for their base classes.

or:

> Subtypes must be substitutable for their base types.

Principle means that a user of a base class should continue to function properly if a derivative of that base class is passed to it. In other words, if some function `User` takes and argument of type `Base`, then it should be legal to pass in an instance of `Derived` to that function:

```csharp
User(new Derived());

static void User(Base b)
{
    Console.WriteLine(b.GetType()); // Derived
}

class Base
{
}

class Derived : Base
{
}
```

The LSP is all about well-designed inheritance. When you inherit from a base class, you must be able to substitute your subclass for that base class without things going terribly wrong. Otherwise, you've used inheritance incorrectly!<sup>1</sup>

## Related links

[↑ Implementing an interface when you don't need one of the properties](https://softwareengineering.stackexchange.com/questions/306105/implementing-an-interface-when-you-dont-need-one-of-the-properties)

<hr>

<sup>1</sup> Head First Object-Oriented Analysis and Design O'Reilly Media; 1st edition (December 19, 2006), page 400
