# Iterator

The **Iterator** is a behavioral software design pattern that provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

## Motivation

An aggregate object such as a list should give you a way to access its elements without exposing its internal structure. Moreover, you might want to traverse the list in different ways, depending on what you want to accomplish. But you probably don't want to bloat the List interface with operations for different traversals, even if you could anticipate the ones you will need. You might also need to have more than one traversal pending on the same list.

The Iterator pattern lets you do all this. The key idea in this pattern is to take the responsibility for access and traversal out of the list object and put it into an iterator object. The Iterator class defines an interface for accessing the list's elements. An iterator object is responsible for keeping track of the current element; that is, it knows which elements have been traversed already.

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

## C# implementation

```csharp
using System;

class Program
{
    public static void Main()
    {
        ICollection<int> collection = new Collection<int>(6);
        collection.AddRange(1, 2, 5, 10, 20, 50);

        IIterator<int> iterator = collection.GetIterator();
        while (iterator.MoveNext())
        {
            Console.WriteLine(iterator.Current);
        }
    }
}

interface ICollection<T> : IAggregate<T>
{
    public void Add(T item);

    public void AddRange(params T[] items);
}

interface IAggregate<T>
{
    IIterator<T> GetIterator();
}

class Collection<T> : ICollection<T>
{
    private readonly T[] _collection;

    private int _currentPos;

    private readonly Lazy<Iterator<T>> _iterator;

    public Collection(int collectionSize)
    {
        _collection = new T[collectionSize];
        _iterator = new Lazy<Iterator<T>>(() => new Iterator<T>(_collection));
    }

    public IIterator<T> GetIterator()
    {
        return _iterator.Value;
    }

    public void Add(T item)
    {
        _collection[_currentPos] = item;
        _currentPos++;
    }

    public void AddRange(params T[] items)
    {
        foreach (T item in items)
        {
            _collection[_currentPos] = item;
            _currentPos++;
        }
    }
}

interface IIterator<out T>
{
    public T Current { get; }

    bool MoveNext();

    void Reset();
}

class Iterator<T> : IIterator<T>
{
    private readonly T[] _collection;

    private int _current = -1;

    public T Current => _collection[_current];

    public Iterator(T[] collection)
    {
        _collection = collection;
    }

    public bool MoveNext()
    {
        if (_current == _collection.Length - 1)
            return false;

        _current++;
        return true;
    }

    public void Reset()
    {
        _current = -1;
    }
}
```

Output:

```output
1
2
5
10
20
50
```
