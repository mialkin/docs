# Singleton

The **Singleton** is a creational software design pattern that ensures a class only has one instance, and provides a global point of access to
it.

## Participants

* **Singleton**
  * defines an Instance operation that lets clients access its unique instance. Instance is a class operation (that is, a class method
in Smalltalk and a static member function in C++).
  * may be responsible for creating its own unique instance.

## C# implementation

```csharp
using System;

class Program
{
    static void Main()
    {
        A a = A.Instance;
        A a2 = A.Instance;

        a.PrintValue();
        a2.PrintValue();

        a.Value = -1;

        a.PrintValue();
        a2.PrintValue();
    }
}

class A
{
    private static A _instance;

    private A()
    {
    }

    public static A Instance
    {
        get
        {
            if (_instance != null) return _instance;

            _instance = new A();
            return _instance;
        }
    }

    public int Value { get; set; } = 1;

    public void PrintValue()
    {
        Console.WriteLine(Value);
    }
}
```

Output:

```output
1
1
-1
-1
```
