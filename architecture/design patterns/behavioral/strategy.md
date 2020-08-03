# Strategy

The **strategy pattern** is a behavioral software design pattern that enables selecting an algorithm at runtime. Instead of implementing a single algorithm directly, code receives run-time instructions as to which in a family of algorithms to use.

The following C# example demonstrates strategy pattern.

```csharp
using System;

public class Program
{
	public static void Main()
	{
		var user = new BehaviorUser(new BehaviorOne());
		user.Behave();
		user.SetBehavior(new BehaviorTwo());
		user.Behave();
	}
}

public interface IBehavior
{
	void Behave();
}

public class BehaviorOne : IBehavior
{
	public void Behave()
	{
		Console.WriteLine("One");
	}
}

public class BehaviorTwo : IBehavior
{
	public void Behave()
	{
		Console.WriteLine("Two");
	}
}

public class BehaviorUser
{
	private IBehavior _behavior;

	public BehaviorUser(IBehavior behavior)
	{
		_behavior = behavior;
	}

	public void Behave()
	{
		_behavior.Behave();
	}

	public void SetBehavior(IBehavior behavior)
	{
		_behavior = behavior;
	}
}
```