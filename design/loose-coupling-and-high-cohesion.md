# Loose coupling and high cohesion

**Loose coupling** refers to minimal dependency among different classes/modules.

**High cohesion** refers to elements within one class/module functionally belonging together and doing one particular thing.

<img src="https://upload.wikimedia.org/wikipedia/commons/0/09/CouplingVsCohesion.svg" width="450px" />

## Application in microservices

> Microservices should be designed around business capabilities, not horizontal layers such as data access or messaging. In addition, they should have loose coupling and high functional cohesion. Microservices are **loosely coupled** if you can change one service without requiring other services to be updated at the same time. A microservice is **cohesive** if it has a single, well-defined purpose, such as managing user accounts or tracking delivery history. A service should encapsulate domain knowledge and abstract that knowledge from clients. For example, a client should be able to schedule a drone without knowing the details of the scheduling algorithm or how the drone fleet is managed<sup>1</sup>.

<hr>

<sup>1</sup> [↑ Using domain analysis to model microservices](https://docs.microsoft.com/en-us/azure/architecture/microservices/model/domain-analysis)
