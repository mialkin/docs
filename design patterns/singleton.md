# Singleton

## C# implementation

```csharp
using System;

class Program
{
    static void Main()
    {
        A a = A.Instance;
        A b = A.Instance;

        a.PrintValue();
        b.PrintValue();

        a.Value = -1;
        
        a.PrintValue();
        b.PrintValue();
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
