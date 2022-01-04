# Domain-Driven Design

The term **domain-driven design** was coined by Eric Evans in his book written in 2003.

DDD provides clean representation of the problem in the code that can be readily understood and verified via tests.

With DDD you "divide and conquer" — by separating a problem into separate subdomains each problem can be tackled intependently.

> "While domain-driven design provides many technical benefits, such as maintainability, it should be applied only to complex domains where the model and the linguistic processes provide clear benefits in the communication of complex information, and in the formulation of a common understanding of the domain." Eric Evans, Domain-Driven design

Some scenarios in which DDD is going to be an overkill:

- Application which doesn't need much more than lots of CRUD logic.
- Application with simple domain but lots of technical challanges.

## Terminology

- **Entity** is a thing that has an identity, i.e. ID.
- **Aggregate** is a DDD pattern for grouping entities together that gives another encapsulation boundary and make easier to do persistence of groups of related things. For example an order with its order items can be constructed as an aggregate.
- **Value object** is a thing that doesn't have an identity which you can compare with other thing by just looking at its properties. A `DateTime` object in .NET can serve as an example. If value object requires validation you put this validation logic in the constructor and then you don't have to check it anywhere else in the code.
- **Domain event** is another DDD pattern.
- **Specification** is another DDD pattern which provides a way to take query logic and take it out of your repositories and put it into your domain model.
