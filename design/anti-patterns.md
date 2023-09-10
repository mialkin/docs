# Anti-patterns

An **anti-pattern** is a common response to a recurring problem that is usually ineffective and risks being highly counterproductive. The term, coined in 1995 by computer programmer Andrew Koenig, was inspired by the 1994 book "Design Patterns: Elements of Reusable Object-Oriented Software".

## Object-oriented programming

**Anemic domain model**: use of the domain model without any business logic. The domain model's objects cannot guarantee their correctness at any moment, because their validation and mutation logic is placed somewhere outside, most likely in multiple places

**God object**: concentrating too many functions in a single class

**Singleton**: carries global state for the duration of the program which can be accessed and modified from anywhere

## Programming

**Hard code**: embedding assumptions about the environment of a system in its implementation

**Magic numbers**: including unexplained numbers in algorithms

**Spaghetti code**: programs whose structure is barely comprehensible, especially because of misuse of code structures

## Software design

**Big ball of mud**: a system with no recognizable structure

## Links

[↑ List of anti-patterns](https://en.wikipedia.org/wiki/Anti-pattern).
