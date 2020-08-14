# Builder

The **builder** is a creational design patterns that separates the construction of a complex object from its representation so that the same construction process can create different representations.

## Participants

* **Builder**
  * specifies an abstract interface for creating parts of a Product
object.
* **ConcreteBuilder**
  * constructs and assembles parts of the product by implementing the
Builder interface.
  * defines and keeps track of the representation it creates.
  * provides an interface for retrieving the product.
* **Director**
  * constructs an object using the Builder interface.
* **Product**
  * represents the complex object under construction. ConcreteBuilder
builds the product's internal representation and defines the process
by which it's assembled.
  * includes classes that define the constituent parts, including
interfaces for assembling the parts into the final result.

## Collaborations

* The client creates the Director object and configures it with the desired
Builder object.
* Director notifies the builder whenever a part of the product should be built.
* Builder handles requests from the director and adds parts to the product.
* The client retrieves the product from the builder.

## C# implementation

```csharp
using System;

class Program
{
    static void Main()
    {
        var a = new BuilderA();
        var director = new Director(a);
        director.Build();
        IProduct product = a.GetProduct();
    }
}

class Director
{
    private IBuilder _builder;

    public Director(IBuilder builder)
    {
        _builder = builder;
    }

    public void Build()
    {
        _builder.BuildStepOne();
        _builder.BuildStepTwo();
        _builder.BuildStepThree();
    }
}

interface IBuilder
{
    void BuildStepOne();

    void BuildStepTwo();

    void BuildStepThree();

    IProduct GetProduct();
}

class BuilderA : IBuilder
{
    private ProductA _product;

    public BuilderA()
    {
        _product = new ProductA();
    }

    public void BuildStepOne()
    {
        Console.WriteLine("One");
    }

    public void BuildStepTwo()
    {
        Console.WriteLine("Two");
    }

    public void BuildStepThree()
    {
        Console.WriteLine("Three");
    }

    public IProduct GetProduct()
    {
        return _product;
    }
}

interface IProduct
{
}

class ProductA : IProduct
{
}
```

Output:

```console
One
Two
Three
```

## See also

[IHostBuilder Interface](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.ihostbuilder)
