# Template Method

The **Template Method** is a behavioral software design pattern that defines the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

## Participants

* **AbstractClass**
  * defines abstract primitive operations that concrete subclasses define to implement steps of an algorithm.
  * implements a template method defining the skeleton of an algorithm. The template method calls primitive operations as well as operations defined in AbstractClass or those of other objects.
* **ConcreteClass**
  * implements the primitive operations to carry out subclass-specific steps of the algorithm.

## Implementation

```csharp
using System;

class Program
{
    public static void Main()
    {
        Abstract one = new ConcreteA();
        one.TemplateMethod();

        Abstract two = new ConcreteB();
        two.TemplateMethod();
    }
}

abstract class Abstract
{
    public void TemplateMethod()
    {
        int result = MethodOne();

        MethodTwo(result);
    }

    protected abstract int MethodOne();

    protected abstract void MethodTwo(int number);
}

class ConcreteA : Abstract
{
    protected override int MethodOne()
    {
        return 10;
    }

    protected override void MethodTwo(int number)
    {
        Console.WriteLine(10 * number);
    }
}

class ConcreteB : Abstract
{
    protected override int MethodOne()
    {
        return 50;
    }

    protected override void MethodTwo(int number)
    {
        Console.WriteLine(number / 10);
    }
}
```
