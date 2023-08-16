# Choreography vs orchestration

*Choreography* and *orchestration* are two different approaches for how to interact with other software components in a microservice architecture. Both have their benefits and drawbacks.

[↑ Orchestration vs Choreography](https://camunda.com/blog/2023/02/orchestration-vs-choreography).

## Choreography

**Choreography** is a design pattern that enables the development of distributed systems where the components interact with each other through a set of well-defined interfaces, without requiring a central authority or coordinator.

A choreographed system, by definition, uses event-driven communication, whereas microservice orchestration uses command-driven communication. An event is something which happened in the past and is a fact. The sender does not know who picks up the event or what happens next.

## Orchestration

**Orchestration** is a design pattern that enables the development of distributed systems where a central authority or coordinator is responsible for managing and coordinating the actions of the different components.

An orchestrated system uses command driven communication. In comparison to the event a command has an intent. The sender wants something to happen and the recipient does not necessarily know who issued the command.

## Links

[↑ Orchestration and choreography in .NET microservices](https://www.infoworld.com/article/3687638/orchestration-and-choreography-in-net-microservices.html).

[↑ Microservices Orchestration vs Choreography: What should you prefer?](https://www.accionlabs.com/microservices-orchestration-vs-choreography-what-to-prefer).

[↑ Microservices Choreography Best Practices and Tools](https://levelup.gitconnected.com/microservices-choreography-best-practices-and-tools-8e46f12bf8f1).

[↑ Orchestration vs Choreography](https://camunda.com/blog/2023/02/orchestration-vs-choreography/).

[↑ Choreography pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/choreography).
