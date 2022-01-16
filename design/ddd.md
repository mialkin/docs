# Domain-Driven Design

- [Domain-Driven Design](#domain-driven-design)
  - [Domain](#domain)
  - [Subdomain](#subdomain)
  - [Domain model](#domain-model)
  - [Bounded context](#bounded-context)
  - [Ubiquitous language](#ubiquitous-language)
  - [Context map](#context-map)
  - [Entity](#entity)
  - [Aggregate](#aggregate)
  - [Value object](#value-object)
  - [Domain event](#domain-event)
  - [Specification](#specification)

The term **domain-driven design** was coined by Eric Evans in his book written in 2003.

DDD provides clean representation of the problem in the code that can be readily understood and verified via tests.

With DDD you "divide and conquer" — by separating a problem into separate subdomains each problem can be tackled intependently.

> "While domain-driven design provides many technical benefits, such as maintainability, it should be applied only to complex domains where the model and the linguistic processes provide clear benefits in the communication of complex information, and in the formulation of a common understanding of the domain." Eric Evans, Domain-Driven design

Some scenarios in which DDD is going to be an overkill:

- Application which doesn't need much more than lots of CRUD logic.
- Application with simple domain but lots of technical challanges.

## Domain

A **domain** can refer to both the entire domain of the business, as well as just one core or supporting area of it. When referring to just one area of the business we will use words *core domain*, *subdomain*, and the like.

## Subdomain

Almost every software domain has multiple **subdomains**.

If it models some aspect of the business that is essential, yet not *core*, it is a **supporting subdomain**.

The business creates a supporting subdomain because it is somewhat specialized. Otherwise, if it captures nothing special to the business, yet is required for the overall business solution, it is a **generic subdomain**. Being supporting or heneric doesn't mean unimportant. These kinds of subdomains are important to the success of the business, yet there is no need for the business to excel in these areas. It's the **core domain** that requires excellence in implementation, since it will provide distinct advantages to the business.

## Domain model

A **domain model** is a software model of the very specific business domain you are working in.

## Bounded context

A **bounded context** is a conceptual boundary where a domain model is applicable. It provides a context for the *ubiquitous language* that is spoken by the team and expressed in its carefully designed software model.

## Ubiquitous language

A **ubiquitous language** is a shared team language. It's shared by domain experts and developers alike. In fact, it's shared by everyone on the project team. No matter your role on the team, since you are on the team you use the ubiquitous language of the project.

Ubiquitous means "pervasive," or "found everywhere," as *spoken among the team and expressed by the single domain model* that the team develops. There is one ubiquitous language per bounded context.

## Context map

A **context map** is a visual representation of the system's bounded contexts and integrations between them.

There are nine context map patterns and three different team relationships which describe the contact between bounded contexts and teams.

Context maps can be used to analyze existing systems or application landscapes but they are also suitable for upfront design considerations.

- [↑ Context mapping](https://github.com/ddd-crew/context-mapping)
- [↑ Context mapping in domain-driven design](https://medium.com/ingeniouslysimple/context-mapping-in-domain-driven-design-9063465d2eb8)

## Entity

An **entity** is an object that has an identity, i.e. ID.

## Aggregate

An **aggregate** is a DDD pattern for grouping entities together that gives another encapsulation boundary and make easier to do persistence of groups of related things. For example an order with its order items can be constructed as an aggregate.

## Value object

A **value object** is an object that doesn't have an identity which you can compare with other thing by just looking at its properties. A `DateTime` object in .NET can serve as an example. If value object requires validation you put this validation logic in the constructor and then you don't have to check it anywhere else in the code.

## Domain event

A **domain event** is another DDD pattern.

## Specification

A **specification** is another DDD pattern which provides a way to take query logic and take it out of your repositories and put it into your domain model.
