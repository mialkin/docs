# The Open–Closed Principle (OCP)

The principle states:

- Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.<sup>1</sup>

Principle means that we should write our modules so that they can be extended, without requiring them to be modified. In other words, we want to be able to change what the modules do, without changing the source code of the modules.

*OCP is achieved by using abstraction.*

Simplier definition:

> You should be able to extend a classes behavior, without modifying it.

## Meyer's open–closed principle

Bertrand Meyer is generally credited for having originated the term open–closed principle, which appeared in his 1988 book Object Oriented Software Construction:

- A module will be said to be open if it is still available for extension. For example, it should be possible to add fields to the data structures it contains, or new elements to the set of functions it performs.

- A module will be said to be closed if [it] is available for use by other modules. This assumes that the module has been given a well-defined, stable description (the interface in the sense of information hiding).

At the time Meyer was writing, adding fields or functions to a library inevitably required changes to any programs depending on that library. Meyer's proposed solution to this dilemma relied on the notion of object-oriented inheritance (specifically implementation inheritance):

- A class is closed, since it may be compiled, stored in a library, baselined, and used by client classes. But it is also open, since any new class may use it as parent, adding new features. When a descendant class is defined, there is no need to change the original or to disturb its clients.

## Polymorphic open–closed principle

During the 1990s, the open–closed principle became popularly redefined to refer to the use of abstracted interfaces, where the implementations can be changed and multiple implementations could be created and polymorphically substituted for each other.

In contrast to Meyer's usage, this definition advocates inheritance from abstract base classes. Interface specifications can be reused through inheritance but implementation need not be. The existing interface is closed to modifications and new implementations must, at a minimum, implement that interface.

## Real life example

Using RabbitMQ's publish/subscribe mechanism makes your communication from the sender sender microservice to be available to additional subscriber microservices or to external applications. Thus, it helps to follow the *open-closed principle* in the sending service. That way, additional subscribers can be added in the future without the need to modify the sender service.

<hr>

<sup>1</sup> Meyer, Bertrand. Object-Oriented Software Construction, 2d ed. Upper Saddle River, NJ: Prentice Hall, 1997.
