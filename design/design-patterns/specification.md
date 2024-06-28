# Specification

The **specification** is a pattern that allows us to encapsulate some piece of domain knowledge into a single unit — specification — and reuse it in different parts of the code base.

Specification aims at two goals:

1\. Avoid domain knowledge duplication. Stick to DRY principle.

2\. Enable declarative approach. Improve code maintainability.

The pattern was introduces by Eric Evans and Martin Fowler in early 2000s in [↑ the paper](https://martinfowler.com/apsupp/spec.pdf).

There are [↑ 4 use cases](https://enterprisecraftsmanship.com/posts/specification-pattern-always-valid-domain-model/) for specification pattern:

- **In-memory validation** — When you need to check that an object in the memory meets the criteria
- **Data retrieval** — When you need to query objects from the database that meet some criteria. Those criteria are called specification
- **Creation of a new object** — When you need to create a new, arbitrary object that meets the criteria. Martin Fowler and Eric Evans called it _construction-to-order_ in their original paper
- **Bulk updates** — When you need to modify a bunch of objects that meet the criteria

## Links

https://github.com/vkhorikov/SpecPattern

https://github.com/daltonbr/SpecificationPattern

[Specification pattern: C# implementation](https://enterprisecraftsmanship.com/posts/specification-pattern-c-implementation/).

[Specification Pattern vs Always-Valid Domain Model](https://enterprisecraftsmanship.com/posts/specification-pattern-always-valid-domain-model/)
