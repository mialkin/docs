# The Single Responsibility Principle (CRP)

The principle states:

> A class should have only one reason to change.<sup>1</sup>

What is a responsibility?

> In the context of the SRP, we define a responsibility to be "a reason for change".<sup>2</sup>

If an application is not changing in ways that casuse the two responsibilities to change at different times, then there is no need to separate them. It is not
wise to apply the SRP if there is no symptoms of possible change in the future.

Following this principle helps to produce more loosely coupled and modular systems, since many kinds of new behavior can be implemented as new classes, rather than by adding additional responsibility to existing classes. Adding new classes is always safer than changing existing classes, since no code yet depends on the new classes.

In a monolithic application, we can apply the single responsibility principle at a high level to the layers in the application. Presentation responsibility should remain in the UI project, while data access responsibility should be kept within an infrastructure project. Business logic should be kept in the application core project, where it can be easily tested and can evolve independently from other responsibilities.

When this principle is applied to application architecture and taken to its logical endpoint, you get microservices. A given microservice should have a single responsibility. If you need to extend the behavior of a system, it's usually better to do it by adding additional microservices, rather than by adding responsibility to an existing one.

<hr>

<sup>1, 2</sup> Robert C. Martin, Agile Software Development (Pearson Education, Inc, 2003), pages 95 and 97.

## Links

[↑ Architectural principles](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles#single-responsibility)
