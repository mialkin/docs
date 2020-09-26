# Iterator

The **Iterator** is a behavioral software design pattern that provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

## Participants

* **Iterator**
  * defines an interface for accessing and traversing elements.
* **ConcreteIterator**
  * implements the Iterator interface.
  * keeps track of the current position in the traversal of the aggregate.
* **Aggregate**
  * defines an interface for creating an Iterator object.
* **ConcreteAggregate**
  * implements the Iterator creation interface to return an instance of the proper ConcreteIterator.
