# `event`

The `event` keyword is used to declare an event in a *publisher class*.

## Table of contents

- [`event`](#event)
  - [Table of contents](#table-of-contents)
  - [General](#general)
  - [`event` vs `delegate`](#event-vs-delegate)

## General

Events enable a class or object to notify other classes or objects when something of interest occurs.

The class that raises the event is called **publisher** and the classes that handle the event are called **subscribers**.

Events are a special kind of multicast [delegate](/csharp/types/delegate.md) that can only be invoked from within the class, or derived classes, or structure where they are declared, i.e. in the publisher class. If other classes or structures subscribe to the event, their event handler methods will be called when the publisher class raises the event.

## `event` vs `delegate`

You can subscribe to `event` and to `delegate`. You can raise `event` and call `delegate`. So why bother with events? You can't call `event` from outside of the class, but you can call `delegate`, if it's public.

You can also overwrite `delegate` with `=` operator, while with `event` you can use only `+=` and `-=`. To remove all subscribers from event you need to set event variable to `null`. A `null` reference is the canonical way of representing an empty invocation list, effectively.

You can write a public method on the class you want the event to fire from and fire the event when it is called. You can then call this method from whatever user of your class. Of course, this ruins encapsulation and is bad design.
