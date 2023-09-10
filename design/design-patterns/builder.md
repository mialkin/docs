# Builder

The **Builder** is a creational software design pattern that separates the construction of a complex object from its representation so that the same construction process can create different representations.

## Participants

* **Builder**
  * specifies an abstract interface for creating parts of a Product object.
* **ConcreteBuilder**
  * constructs and assembles parts of the product by implementing the Builder interface.
  * defines and keeps track of the representation it creates.
  * provides an interface for retrieving the product.
* **Director**
  * constructs an object using the Builder interface.
* **Product**
  * represents the complex object under construction. ConcreteBuilder builds the product's internal representation and defines the process
by which it's assembled.
  * includes classes that define the constituent parts, including
interfaces for assembling the parts into the final result.

## Collaborations

* The client creates the Director object and configures it with the desired Builder object.
* Director notifies the builder whenever a part of the product should be built.
* Builder handles requests from the director and adds parts to the product.
* The client retrieves the product from the builder.

## C# implementation

```csharp
using System;

IBuilder a = new BuilderA();
IProduct product = Director(a);
Console.WriteLine(product.Name);

IProduct Director(IBuilder b)
{
    b.BuildStepOne();
    b.BuildStepTwo();
    b.BuildStepN();
    
    return b.Build();
}

interface IBuilder
{
    void BuildStepOne();

    void BuildStepTwo();

    void BuildStepN();

    IProduct Build();
}

class BuilderA : IBuilder
{
    private readonly ProductA _product;

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

    public void BuildStepN()
    {
        _product.Name = "Product";
        Console.WriteLine("N");
    }

    public IProduct Build()
    {
        return _product;
    }
}

interface IProduct
{
    public string Name { get; set; }
}

class ProductA : IProduct
{
    public string Name { get; set; } 
}
```

Output:

```output
One
Two
N
Product
```
