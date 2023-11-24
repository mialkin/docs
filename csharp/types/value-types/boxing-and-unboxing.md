# Boxing and unboxing

The **boxing** is the process of converting a [value type](value-types.md) to the type `object` or to any interface type implemented by this value type.

When the CLR boxes a value type, it wraps the value inside a `System.Object` instance and stores it on the managed heap. Unboxing extracts the value type from the object. Boxing is implicit; unboxing is explicit. The concept of boxing and unboxing underlies the C# unified view of the type system in which a value of any type can be treated as an object.

## Performance

It is best to avoid using value types in situations where they must be boxed a high number of times, for example in non-generic collections classes such as [↑ `ArrayList`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.arraylist). You can avoid boxing of value types by using generic collections such as `List<T>`. Boxing and unboxing are computationally expensive processes. When a value type is boxed, an entirely new object must be created. This can take up to 20 times longer than a simple reference assignment. When unboxing, the casting process can take four times as long as an assignment.

In generics, such as `List<T>`, value types are still stored on the heap. The difference is that, internally, a `List<int>` makes a single array of integers, and can store the numbers directly. With `ArrayList` you end up storing an array of references to boxed integer values.

[↑ Boxing and Unboxing](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/boxing-and-unboxing).
