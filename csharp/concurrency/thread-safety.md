# Thread safety

A **thread safety** is an ability of a piece of code, typically a function, object, or data structure, to be safely accessed and manipulated by multiple threads concurrently without causing unexpected or erroneous behavior.

Thread safety is achieved primarily with locking and by reducing the possibilities for thread interaction.

General-purpose types are rarely thread-safe in their entirety. Thread safety is hence usually implemented just where it needs to be, in order to handle a specific multithreading scenario.

## Table of contents

- [Thread safety](#thread-safety)
  - [Table of contents](#table-of-contents)
  - [Thread safety and .NET framework types](#thread-safety-and-net-framework-types)
    - [Static methods](#static-methods)
    - [Read-only thread safety](#read-only-thread-safety)
  - [Rich client applications and thread affinity](#rich-client-applications-and-thread-affinity)

## Thread safety and .NET framework types

Nearly all of .NET Framework non-primitive types are not thread-safe (for anything more than read-only access) when instantiated, and yet they can be used in multithreaded code if all access to any given object is protected via a lock.

Enumerating .NET collections is also thread-unsafe in the sense that an exception is thrown if the list is modified during enumeration. Rather than locking for the duration of enumeration, we can first copy the items to an array. This avoids holding the lock excessively if what we're doing during enumeration is potentially time-consuming. Another solution is to use a reader/writer lock.

### Static methods

Imagine if the static property on the `DateTime` struct, `DateTime.Now`, was not thread-safe, and that two concurrent calls could result in garbled output or an exception. The only way to remedy this with external locking might be to lock the type itself — `lock(typeof(DateTime))` — before calling `DateTime.Now`. This would work only if all programmers agreed to do this (which is unlikely). Furthermore, locking a type creates problems of its own.

For this reason, static members on the `DateTime` struct have been carefully programmed to be thread-safe. This is a common pattern throughout the .NET Framework: *static members are thread-safe; instance members are not*. Following this pattern also makes sense when writing types for public consumption, so as not to create impossible thread-safety conundrums.

Thread safety in static methods is something that you must explicitly code: it doesn't happen automatically by virtue of the method being static!

### Read-only thread safety

Making types thread-safe for concurrent read-only access (where possible) is advantageous because it means that consumers can avoid excessive locking. Many of the .NET Framework types follow this principle: collections, for instance, are thread-safe for concurrent readers.

Following this principle yourself is simple: if you document a type as being thread-safe for concurrent read-only access, don't write to fields within methods that a consumer would expect to be read-only (or lock around doing so). For instance, in implementing a `ToArray()` method in a collection, you might start by compacting the collection's internal structure. However, this would make it thread-unsafe for consumers that expected this to be read-only.

Read-only thread safety is one of the reasons that enumerators are separate from "enumerables": two threads can simultaneously enumerate over a collection because each gets a separate enumerator object.

In the absence of documentation, it pays to be cautious in assuming whether a method is read-only in nature. A good example is the [↑ `Random`](https://learn.microsoft.com/en-us/dotnet/api/system.random) class: when you call [↑ `Random.Next()`](https://learn.microsoft.com/en-us/dotnet/api/system.random.next), its internal implementation requires that it update private seed values. Therefore, you must either lock around using the `Random` class, or maintain a separate instance per thread. Or better use thread-safe instance of `Random` class obtained from [↑ `Random.Shared`](https://learn.microsoft.com/en-us/dotnet/api/system.random.shared) property.

## Rich client applications and thread affinity

Both the WPF and Windows Forms libraries follow models based on thread affinity. Although each has a separate implementation, they are both very similar in how they function.

The objects that make up a rich client are based primarily on [↑ `DependencyObject`](https://learn.microsoft.com/en-us/dotnet/api/system.windows.dependencyobject) in the case of WPF, or [↑ `Control`](https://learn.microsoft.com/en-us/dotnet/api/system.windows.forms.control) in the case of Windows Forms. These objects have *thread affinity*, which means that only the thread that instantiates them can subsequently access their members. Violating this causes either unpredictable behavior, or an exception to be thrown.
