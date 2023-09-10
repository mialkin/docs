# State

The **State** is is a behavioral software design pattern that allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

## Participants

* **Context**
  * defines the interface of interest to clients.
  * maintains an instance of a ConcreteState subclass that defines the current state.
* **State**
  * defines an interface for encapsulating the behavior associated with a particular state of the Context.
* **ConcreteState subclasses**
  * each subclass implements a behavior associated with a state of the Context.

## C# implementation

```csharp
using System;

class Program
{
    public static void Main()
    {
        IState stateOne = new StateOne();
        IState stateTwo = new StateTwo();
        IState stateThree = new StateThree();

        stateOne.SetNextState(stateTwo);
        stateTwo.SetNextState(stateThree);

        IContext context = new Context(stateOne);

        stateOne.SetContext(context);
        stateTwo.SetContext(context);
        stateThree.SetContext(context);

        context.Request();
        context.Request();
        context.Request();
        context.Request();
    }
}

interface IContext
{
    void Request();
    void SetState(IState state);
}

class Context : IContext
{
    private IState _state;

    public Context(IState state)
    {
        _state = state;
    }

    public void SetState(IState state)
    {
        _state = state;
    }

    public void Request()
    {
        _state.Handle();
    }
}

interface IState
{
    void SetNextState(IState state);
    void SetContext(IContext context);
    void Handle();
}

class StateOne : IState
{
    private IContext _context;
    private IState _nextState;

    public void SetNextState(IState nextState)
    {
        _nextState = nextState;
    }

    public void SetContext(IContext context)
    {
        _context = context;
    }

    public void Handle()
    {
        Console.WriteLine("Request was handled by state 1");
        _context.SetState(_nextState);
    }
}

class StateTwo : IState
{
    private IContext _context;
    private IState _nextState;

    public void SetNextState(IState nextState)
    {
        _nextState = nextState;
    }

    public void SetContext(IContext context)
    {
        _context = context;
    }

    public void Handle()
    {
        Console.WriteLine("Request was handled by state 2");
        _context.SetState(_nextState);
    }
}

class StateThree : IState
{
    public void SetNextState(IState state)
    {
    }

    public void SetContext(IContext context)
    {
    }

    public void Handle()
    {
        Console.WriteLine("Request was handled by state 3");
    }
}
```

Output:

```output
Request was handled by state 1
Request was handled by state 2
Request was handled by state 3
Request was handled by state 3
```
