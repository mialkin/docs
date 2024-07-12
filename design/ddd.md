# Domain-driven design

The term **domain-driven design** was coined by Eric Evans in his book written in 2003.

DDD provides clean representation of the problem in the code that can be readily understood and verified via tests.

With DDD you "divide and conquer" — by separating a problem into separate [subdomains](#subdomain) each problem can be tackled interdependently.

"While domain-driven design provides many technical benefits, such as maintainability, it should be applied only to complex domains where the model and the linguistic processes provide clear benefits in the communication of complex information, and in the formulation of a common understanding of the domain."  — Eric Evans, Domain-Driven design

Some scenarios in which DDD is going to be an overkill:

- Application which doesn't need much more than lots of CRUD logic
- Application with simple domain but lots of technical challenges

## Table of contents

- [Domain-driven design](#domain-driven-design)
  - [Table of contents](#table-of-contents)
  - [Domain](#domain)
    - [Subdomain](#subdomain)
    - [Domain model](#domain-model)
      - [Anemic domain model](#anemic-domain-model)
      - [Rich domain model](#rich-domain-model)
    - [Bounded context](#bounded-context)
      - [Ubiquitous language](#ubiquitous-language)
      - [Context map](#context-map)
    - [Domain event](#domain-event)
  - [Entity](#entity)
    - [Aggregate](#aggregate)
  - [Value object](#value-object)
  - [Event storming](#event-storming)
  - [Specification](#specification)
  - [Videos](#videos)

## Domain

A **domain** can refer to both the entire domain of the business, as well as just one core or supporting area of it. When referring to just one area of the business we will use words *core domain*, [subdomain](#subdomain), and the like.

### Subdomain

Almost every software [domain](#domain) has multiple **subdomains**.

If it models some aspect of the business that is essential, yet not *core*, it is a **supporting subdomain**.

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

Strive for *rich* domain models and have an awareness of the strengths and weaknesses of those that are not so rich.

### Bounded context

A **bounded context** is a conceptual boundary where a [domain model](#domain-model) is applicable. It provides a context for the [ubiquitous language](#ubiquitous-language) that is spoken by the team and expressed in its carefully designed software model.

#### Ubiquitous language

A **ubiquitous language** is a shared team language. It's shared by domain experts and developers alike. In fact, it's shared by everyone on the project team. No matter your role on the team, since you are on the team you use the ubiquitous language of the project.

Ubiquitous means "pervasive," or "found everywhere," as *spoken among the team and expressed by the single domain model* that the team develops.

There is one ubiquitous language per [bounded context](#bounded-context).

#### Context map

A **context map** is a visual representation of the system's [bounded contexts](#bounded-context) and integrations between them.

There are nine context map patterns and three different team relationships which describe the contact between bounded contexts and teams.

Context maps can be used to analyze existing systems or application landscapes but they are also suitable for upfront design considerations.

- [↑ Context mapping](https://github.com/ddd-crew/context-mapping)
- [↑ Context mapping in domain-driven design](https://medium.com/ingeniouslysimple/context-mapping-in-domain-driven-design-9063465d2eb8)

### Domain event

A **domain event** is an occurrence of something that happened in the [domain](#domain).

## Entity

An **entity** is an object that has an identity, i.e. an ID.

### Aggregate

An **aggregate** is a DDD pattern for grouping [entities](#entity) together that gives another encapsulation boundary and make easier to do persistence of groups of related things. For example an order with its order items can be constructed as an aggregate.

## Value object

A **value object** is an immutable type that is distinguishable only by the state of its properties. That is, unlike an [entity](#entity), which has a unique identifier and remains distinct even if its properties are otherwise identical, two value objects with the exact same properties can be considered equal.

## Event storming

An **event storming** is a workshop-based method to quickly find out what is happening in the domain of a software program

## Specification

A **specification** is another DDD pattern which provides a way to take query logic and take it out of your repositories and put it into your domain model.

## Videos

[↑ Domain-Driven Refactoring - Jimmy Bogard - NDC Porto 2022](https://www.youtube.com/watch?v=gxgKgMvPH9I).

[↑ Чистая архитектура и Domain-Driven Design](https://www.youtube.com/watch?v=fx6NWIgjH7w).
