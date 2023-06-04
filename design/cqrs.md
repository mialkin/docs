# CQRS

**Command (and) Query Responsibility Segregation** or **CQRS** is a  a pattern that separates read and update operations for a data store.

CQRS is when only command can change the model, query can only read it. A model here in the DDD sense, that is, the model of some subject area.

The term CQRS was coined by Greg Young.

[↑ CQRS Documents by Greg Young](https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf).

## Key points

- CQRS is simple
- CQRS has many advantages compared to traditional service approach
- CQRS fits for different kinds of projects: you can use it for different stacks and in different domains

## CQRS and event sourcing

> Everyone always talks about CQRS and event sourcing when, really, it's event sourcing and CQRS. When I first started teaching people about CQRS and event sourcing it was advantageous to teach them CQRS first, and then teach them event sourcing. You can use CQRS without event sourcing, but with event sourcing you must use CQRS.

[↑ Greg Young - CQRS and Event Sourcing - Code on the Beach 2014](https://youtu.be/JHGkaShoyNs?t=60).

> I have been talking about CQRS and event source subjects for a very, very long time. The first time I talked about this was in 2006 at Q Con San Francisco.

## Links

- [↑ CQRS pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs)
- [↑ А какие виды CQRS вы знаете?](https://www.youtube.com/watch?v=TnS6PwxHcLg)