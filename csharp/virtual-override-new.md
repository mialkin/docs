# Extend `virtual` method with `override`. Hide method with `new`

## Table of contents

- [Extend `virtual` method with `override`. Hide method with `new`](#extend-virtual-method-with-override-hide-method-with-new)
  - [Table of contents](#table-of-contents)
  - [`virtual`, `override`](#virtual-override)
  - [`new`](#new)

## `virtual`, `override`

The `override` modifier *extends* the base class `virtual` method.

```csharp
var @base = new Base();
var derived = new Derived();
Base baseDerived = new Derived();

@base.Run();
derived.Run();
baseDerived.Run();

public class Base
{
    public virtual void Run()
    {
        Console.WriteLine("Base");
    }
}

public class Derived : Base
{
    public override void Run()
    {
        Console.WriteLine("Derived");
    }
}

// Output:
// Base
// Derived
// Derived
```

## `new`

The `new` modifier *hides* an accessible base class method.

```csharp
var @base = new Base();
var derived = new Derived();
Base baseDerived = new Derived();

@base.Run();
derived.Run();
baseDerived.Run();

public class Base
{
    public void Run()
    {
        Console.WriteLine("Base");
    }
}

public class Derived : Base
{
    public new void Run()
    {
        Console.WriteLine("Derived");
    }
}

// Output:
// Base
// Derived
// Base
```

Code above would work exactly the same way if you remove the `new` keyword. In that case the only difference will be that you will get the following compiler warning next to `Run` method of `Derived` class: `The keyword 'new' is required on 'Run' because it hides method 'void Base.Run()'`.

The `new` keyword suppresses the warning. By using `new`, you are asserting that you are aware that the member that it modifies hides a member that is inherited from the base class.

[↑ virtual, new and override in C#](https://pnguyen.io/posts/virtual-new-override-csharp/).
