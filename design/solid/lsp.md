# The Liskov Subsititution Principles (LSP)

The principle states:

> Subclasses should be substituted for their base classes.

Principle means that a user of a base class should continue to function properly if a derivative of that base class is passed to it. In other words, if some function `User` takes and argument of type `Base`, then it should be legal to pass in an instance of `Derived` to that function:

```csharp
using System;

public class Program
{
    public static void Main()
    {
        User(new Base());
        User(new Derived());
    }

    static void User(Base b)
    {
        Console.WriteLine(b.GetType());
    }
}

class Base
{
}

class Derived : Base
{
}
```

Output:

```output
Base
Derived
```
