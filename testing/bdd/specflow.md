# SpecFlow

[↑ SpecFlow](https://specflow.org) is a free and open source BDD framework that provides test automation for .NET, based on the Gherkin specification language.

## Table of contents

- [SpecFlow](#specflow)
  - [Table of contents](#table-of-contents)
  - [Hooks](#hooks)
    - [Tag Scoping](#tag-scoping)
  - [Scoped bindings](#scoped-bindings)
  - [Context injection](#context-injection)

## Hooks

Hooks are event bindings that can be used to perform additional automation logic at specific times, such as any setup required prior to executing a scenario. In order to use hooks, you need to add the Binding attribute to your class:

```csharp
[Binding]
public class MyClass
{
    ...
}
```

Hooks are global, but can be restricted to run only for features or scenarios by defining a _scoped binding_, which can be filtered with tags. The execution order of hooks for the same type is undefined, unless specified explicitly.

[↑ Hooks documentation](https://docs.specflow.org/projects/specflow/en/latest/Bindings/Hooks.html).

### Tag Scoping

Most hooks support tag scoping. Use tag scoping to restrict hooks to only those features or scenarios that have at least one of the tags in the tag filter (tags are combined with OR). You can specify the tag in the attribute or using scoped bindings.

## Scoped bindings

Bindings (step definitions, hooks) are global for the entire SpecFlow project. This means that step definitions bound to a very generic step text (e.g. "When I save the changes") become challenging to implement. The general solution for this problem is to phrase the scenario steps in a way that the context is clear (e.g. "When I save the book details").

In some cases however, it is necessary to restrict when step definitions or hooks are executed based on certain conditions. SpecFlow's [↑ scoped bindings](https://docs.specflow.org/projects/specflow/en/latest/Bindings/Scoped-Step-Definitions.html) can be used for this purpose.

```csharp
[Binding]
[Scope(Feature = "Feature name to which this hooks applies")]
public class Hooks
{
    // ...
}
```

[↑ Scoped bindings documentation](https://docs.specflow.org/projects/specflow/en/latest/Bindings/Scoped-Step-Definitions.html).

## Context injection

SpecFlow supports a very simple dependency framework that is able to instantiate and inject class instances for scenarios. This feature allows you to group the shared state in context classes, and inject them into every binding class that needs access to that shared state.

To use context injection:

- Create your POCOs (plain old CLR object), simple .NET classes, representing the shared data.
- Define them as constructor parameters in every binding class that requires them.
- Save the constructor argument to instance fields, so you can use them in the step definitions.

Rules:

- The life-time of these objects is limited to a scenario's execution.
- If the injected objects implement `IDisposable`, they will be disposed after the scenario is executed.
- The injection is resolved recursively, i.e. the injected class can also have dependencies.
- Resolution is done using public constructors only.
- If there are multiple public constructors, SpecFlow takes the first one.

Example:

```csharp
public class Context
{
    public string OrderNumber { get; set; }
}

[Binding]
public class Given
{
    private readonly Context _context;

    public Given(Context context)
    {
        _context = context;
    }

    [Given]
    public async Task GivenOrderSubmitted()
    {
        // Use _context.OrderNumber here.
    }
}
```

[↑ Context injection documentation](https://docs.specflow.org/projects/specflow/en/latest/Bindings/Context-Injection.html).
