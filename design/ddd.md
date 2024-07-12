# Domain-driven design

The **domain-driven design**, or **DDD** for short, is a software design strategy intended to take complex [domains](#domain) and simplify them into a extensible and maintainable software solution.

The domain-driven design was introduced by Eric Evans in his book written in 2003.

## Table of contents

- [Domain-driven design](#domain-driven-design)
  - [Table of contents](#table-of-contents)
  - [Area of application](#area-of-application)
  - [Domain](#domain)
    - [Subdomain](#subdomain)
    - [Domain model](#domain-model)
      - [Anemic domain model](#anemic-domain-model)
      - [Rich domain model](#rich-domain-model)
    - [Bounded context](#bounded-context)
      - [Ubiquitous language](#ubiquitous-language)
      - [Context map](#context-map)
      - [Shared kernel](#shared-kernel)
      - [Anti-corruption layer](#anti-corruption-layer)
      - [Entity](#entity)
      - [Aggregate](#aggregate)
      - [Value object](#value-object)
    - [Domain event](#domain-event)
  - [Event storming](#event-storming)
  - [Specification](#specification)
  - [Videos](#videos)

## Area of application

DDD is not a silver bullet.

There are different kinds of projects and DDD is applicable to only fraction of them. The most important attributes of any project are:

- **Amount of data project operates** (single database vs big data)
- **Performance requirements** (simple utility tool vs online trading platform, which usually has requirement to return any call with no more than tens of milliseconds time-frame)
- **Business logic complexity** (CRUD application vs ERP system with lots of business rules)
- **Technical complexity** (complexity of algorithms needed to be implemented to make software work, for example, a low level programming for embedded systems where you need to deal with many of hardware systems manually)

The techniques DDD proposes are useful _if and only if_ the project you are working on has a lot of business rules. DDD won't help you if you work with big data, or need to achieve and outstanding performance, or program against hardware systems.

The only purpose DDD concepts serve is to _tackle business logic complexity_. So, if you need to create an [↑ x.com](https://x.com/)-like application domain-driven design won't help you much with that, because the challenge in this type of software comes out not from its business rules. The business logic itself is pretty simple in [↑ X](https://en.wikipedia.org/wiki/Twitter). What makes it hard to implement is the great performance and scalability requirements.

A typical example of software with complicated business logic is enterprise-level applications. It's true that most of the enterprise projects don't have outstanding performance requirements, they operate moderate amounts of data, and developers working on them usually don't have to deal with technical complexity by their own, because there are plenty of tools, that abstract out this kind of complexity for them. The biggest challenge in such projects is to handle business logic complexity in such a way that it would be possible to extend and maintain the solution in the long run. That is exactly the task the DDD practices are aimed to solve. They help us create code which not only fully covers the problem, but also does it in the simplest and, thus, the most maintainable way possible.

> "While domain-driven design provides many technical benefits, such as maintainability, it should be applied only to complex domains where the model and the linguistic processes provide clear benefits in the communication of complex information, and in the formulation of a common understanding of the domain."
>
> — Eric Evans, Domain-Driven design

## Domain

A **domain** can refer to both the entire domain of the business, as well as just one core or supporting area of it. When referring to just one area of the business we will use words _core domain_, [subdomain](#subdomain), and the like.

### Subdomain

Almost every software [domain](#domain) has multiple **subdomains**.

If it models some aspect of the business that is essential, yet not _core_, it is a **supporting subdomain**.

The business creates a supporting subdomain because it is somewhat specialized. Otherwise, if it captures nothing special to the business, yet is required for the overall business solution, it is a **generic subdomain**. Being supporting or generic doesn't mean unimportant. These kinds of subdomains are important to the success of the business, yet there is no need for the business to excel in these areas. It's the **core domain** that requires excellence in implementation, since it will provide distinct advantages to the business.

Recognizing anemic domains:

- Looks like the real thing with objects named for nouns in the domain
- Little or no behavior
- Equate to property bags with getters and setters
- All business logic has been relegated to service objects

### Domain model

A **domain model** is a software model of the very specific business [domain](#domain) you are working in.

#### Anemic domain model

An **anemic domain model** is a [domain model](#domain-model) that has a state but lacks behavior.

Some kinds of objects, such as data transfer objects, DTOs, are expected to simply be a collection of data. However, the objects that model the behavior of the problem space within the application should use encapsulation to manage their internal state and behavior. An anemic model frequently fails to follow the [↑ Tell, Don't Ask](https://deviq.com/principles/tell-dont-ask) principle, since objects cannot perform operations on their own state, but are constantly manipulated by other objects in a non-object-oriented fashion. Rich domain models provide useful behavior to clients within the system. Some code smells that may indicate an anemic domain model include [↑ exposed collection properties](https://deviq.com/antipatterns/exposing-collection-properties), and over-use of properties in general, especially setters.

There is nothing wrong with anemic classes when all you need to do is some CRUD logic. But if you are creating a domain model, you've already made a decision that your domain is too complex for simple CRUD. So, anemia in domain model is considered an anti-pattern.

#### Rich domain model

A **rich domain model** is a [domain model](#domain-model) that represents behaviors and business logic of the domain. Classes that simply affect state are considered anti-patterns in the domain model.

Strive for _rich_ domain models and have an awareness of the strengths and weaknesses of those that are not so rich.

### Bounded context

A **bounded context** is a conceptual boundary where a [domain model](#domain-model) is applicable. It provides a context for the [ubiquitous language](#ubiquitous-language) that is spoken by the team and expressed in its carefully designed software model.

#### Ubiquitous language

A **ubiquitous language** is a shared team language.

It's shared by domain experts and developers alike. In fact, it's shared by everyone on the project team. No matter your role on the team, since you are on the team you use the ubiquitous language of the project.

Ubiquitous means "pervasive," or "found everywhere," as _spoken among the team and expressed by the single domain model_ that the team develops.

There is one ubiquitous language per [bounded context](#bounded-context).

Let's say, for example, you have developed a sales system. In this system you have a class called `Product` which is an atomic sell unit. Let's also say that domain experts refer to this element as both a "product" and a "package". In this case you should call attention to this disparity and suggest to use a single term to avoid confusion.

The concept of ubiquitous language also means you should keep your code base in sync with this single terminology and name all of your classes and table in database after the terms in the ubiquitous language.

#### Context map

A **context map** is a visual representation of the system's [bounded contexts](#bounded-context) and integrations between them.

There are nine context map patterns and three different team relationships which describe the contact between bounded contexts and teams.

Context maps can be used to analyze existing systems or application landscapes but they are also suitable for upfront design considerations.

- [↑ Context mapping](https://github.com/ddd-crew/context-mapping)
- [↑ Context mapping in domain-driven design](https://medium.com/ingeniouslysimple/context-mapping-in-domain-driven-design-9063465d2eb8)

#### Shared kernel

A [↑ shared kernel](https://deviq.com/domain-driven-design/shared-kernel) is a special type of [bounded context](#bounded-context) that contains code and data shared across multiple bounded contexts within the same [domain](#domain).

It acts as a central repository for [ubiquitous language](#ubiquitous-language) elements, domain logic, and data structures that are common to all or a subset of the bounded contexts.

#### Anti-corruption layer

An **anti-corruption layer** is a set of defensive patterns placed between the [domain model](#domain-model) and other [bounded contexts](#bounded-context) or third party dependencies.

The intent of this layer is to prevent the intrusion of foreign concepts and models into the domain model.

If two bounded contexts are hosted within the _same process_ and there is no anti-corruption layer, entities in one bounded context can just directly call entities' methods in another context. Also communication can be performed via domain events. In case there is an anti-corruption layer you can't allow entities in one bounded context just call entities in another bounded context. You must use anti-corruption layer as a proxy, which will handle all translations for entities.

If two bounded contexts are hosted in _separate processes_, i.e. in separate microservices, the communication goes through the network: direct calls via REST and events via message queues. In these situation you don't have to create an anti-corruption layer between the two bounded contexts, because these messaging mechanisms essentially act as such.

#### Entity

An **entity** is an object that has some intrinsic identity, i.e. an ID.

#### Aggregate

An **aggregate** is a DDD pattern for grouping [entities](#entity) together that gives another encapsulation boundary and make easier to do persistence of groups of related things. For example an order with its order items can be constructed as an aggregate.

#### Value object

A **value object** is an immutable type that is distinguishable only by the state of its properties.

That is, unlike an [entity](#entity), which has a unique identifier and remains distinct even if its properties are otherwise identical, two value objects with the exact same properties can be considered equal.

### Domain event

A **domain event** is an occurrence of something that happened in the [domain](#domain).

## Event storming

An **event storming** is a workshop-based method to quickly find out what is happening in the domain of a software program

## Specification

A **specification** is another DDD pattern which provides a way to take query logic and take it out of your repositories and put it into your domain model.

## Videos

[↑ Domain-Driven Refactoring - Jimmy Bogard - NDC Porto 2022](https://www.youtube.com/watch?v=gxgKgMvPH9I).

[↑ Чистая архитектура и Domain-Driven Design](https://www.youtube.com/watch?v=fx6NWIgjH7w).
