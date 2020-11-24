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
using System;

class Program
{
    public static void Main()
    {
        var receiver = new Receiver();
        var command = new Command(receiver);

        Invoker(command);
    }

    private static void Invoker(ICommand command)
    {
        command.Execute(10);
    }
}

interface ICommand
{
    void Execute(int parameter);
}

class Command : ICommand
{
    private readonly IReceiver _receiver;

    public Command(IReceiver receiver)
    {
        _receiver = receiver;
    }

    public void Execute(int parameter)
    {
        _receiver.Act(parameter);
    }
}

interface IReceiver
{
    void Act(int parameter);
}

class Receiver : IReceiver
{
    public void Act(int parameter)
    {
        Console.WriteLine($"Receiver acts with {parameter}.");
    }
}
```

Output:

```output
Receiver acts with 10.
```
