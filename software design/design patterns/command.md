# Command

The **Command** is a behavioral software design pattern that encapsulates a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.

## Participants

* **Command**
  * declares an interface for executing an operation.
* **ConcreteCommand**
  * defines a binding between a Receiver object and an action.
  * implements Execute by invoking the corresponding operation(s) on Receiver.
* **Client**
  * creates a ConcreteCommand object and sets its receiver.
* **Invoker**
  * asks the command to carry out the request.
* **Receiver**
  * knows how to perform the operations associated with carrying out a request. Any class may serve as a Receiver.

## C# implementation

```csharp
```

Output:

```output
```
