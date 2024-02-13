# BDD, Gherkin, SpecFlow

## Table of contents

- [BDD, Gherkin, SpecFlow](#bdd-gherkin-specflow)
  - [Table of contents](#table-of-contents)
  - [BDD](#bdd)
    - [Tools](#tools)
  - [Gherkin](#gherkin)
    - [Feature](#feature)
    - [Scenario](#scenario)
    - [Step](#step)
    - [Tag](#tag)
    - [Descriptions](#descriptions)
    - [Asterisk](#asterisk)
    - [Localization](#localization)
  - [SpecFlow](#specflow)
    - [Hooks](#hooks)
      - [Tag Scoping](#tag-scoping)
    - [Scoped bindings](#scoped-bindings)
    - [Context injection](#context-injection)

## BDD

**Behavior-driven development** or **BDD** is an agile software development process that combines the general techniques and principles of TDD with ideas from domain-driven design and object-oriented analysis and design to provide software development and management teams with shared tools and a shared process to collaborate on software development.

### Tools

[↑ Cucumber](https://github.com/cucumber/cucumber-ruby) is a software tool that supports behavior-driven development. Central to the Cucumber BDD approach is its ordinary language parser called *Gherkin*. It allows expected software behaviors to be specified in a logical language that customers can understand. It is often used for testing other software. It runs automated *acceptance tests* written in a BDD style.

[↑ SpecFlow](https://specflow.org) is a free and open source BDD framework that provides test automation for .NET, based on the Gherkin specification language.

[↑ Xunit.Gherkin.Quick](https://github.com/ttutisani/Xunit.Gherkin.Quick) is a lightweight BDD test framework that targets .NET Standard. It parses Gherkin language and executes xUnit tests corresponding to scenarios.

## Gherkin

**Gherkin** is a language for writing behavior scenarios.

Gherkin's code is written into *feature files* which have `.feature` extension.

The official Gherkin language standard is maintained by [↑ Cucumber](https://cucumber.io), one of the most prevalent BDD automation frameworks. Most other BDD frameworks use Gherkin, but some may not conform 100% to Cucumber's language standards.

Gherkin uses a set of special *keywords* to give structure and meaning to executable specifications.

Either spaces or tabs may be used for indentation. The recommended indentation level is two spaces.

### Feature

A **feature** is an abstraction that helps to group related *scenarios*.

The first primary keyword in a Gherkin document must always be the `Feature`, followed by a `:` and a short text that describes the feature.

```gherkin
Feature: Guess the word

  The word guess game is a turn-based game for two players.
  The Maker makes a word for the Breaker to guess. The game
  is over when the Breaker guesses the Maker's word.

  Scenario: Maker starts a game
```

### Scenario

A **scenario** is a concrete example that illustrates a business rule. It consists of one or more *steps*.

The keyword `Scenario` is a synonym of the keyword `Example`.

### Step

A **step** is a way to map a Gherkin sentence in the specification to a method in the code.

Each step starts with one of the following keywords: `Given`, `When`, `Then`, `And`, `But`.

A BDD framework executes each step in a scenario one at a time, in the sequence they have been written them in. When a framework tries to execute a step, it looks for a matching step definition to execute.

Keywords are not taken into account when looking for a step definition. This means you cannot have a `Given`, `When`, `Then`, `And` or `But` step with the same text as another step.

### Tag

A **tag** is a marker that can be assigned to a feature or a scenario. Assigning a tag to a feature is equivalent to assigning the tag to all scenarios in the feature file.

You can use tags to filter and group generated from specification test methods. For example, you can tag crucial tests with `@important` tag, and then execute these tests more frequently.

### Descriptions

Free-form descriptions can be placed underneath `Feature`, `Example`, `Scenario`, `Background`, `Scenario Outline` and `Rule`.

You can write anything you like, as long as no line starts with a keyword.

### Asterisk

Gherkin also supports using an asterisk `*` in place of any of the normal step keywords. This can be helpful when you have some steps that are effectively a list of things, so you can express it more like bullet points where otherwise the natural language of `And` etc might not read so elegantly.

For example:

```gherkin
Scenario: All done
  Given I am out shopping
  And I have eggs
  And I have milk
  And I have butter
  When I check my list
  Then I don't need anything
```

Could be expressed as:

```gherkin
Scenario: All done
  Given I am out shopping
  * I have eggs
  * I have milk
  * I have butter
  When I check my list
  Then I don't need anything
```

### Localization

Gherkin keywords have translations into multiple languages. To improve readability and flow, most languages have more than one translation for any given keyword.

| English           | Russian                                                 |
| ----------------- | ------------------------------------------------------- |
| `Feature`         | `Функция`, `Функциональность`, `Функционал`, `Свойство` |
| `Background`      | `Предыстория`, `Контекст`                               |
| `Scenario`        | `Пример`, `Сценарий`                                    |
| `Scenario Outline` | `Структура сценария`, `Шаблон сценария`                 |
| `Examples`        | `Примеры`                                               |
| `Given`           | `*`, `Допустим`, `Дано`, `Пусть`                        |
| `When`            | `*`, `Когда`, `Если`                                    |
| `Then`            | `*`, `То`, `Затем`, `Тогда`                             |
| `And`             | `*`, `И`, `К тому же`, `Также`                          |
| `But`             | `*`, `Но`, `А`, `Иначе`                                 |
| `Rule`            | `Правило`                                               |

To use language different from English add `# language: XX` comment at the first line of `.feature` file. Example for Russian language:

```gherkin
# language: ru
```

## SpecFlow

[↑ SpecFlow](https://specflow.org) is a free and open source BDD framework that provides test automation for .NET, based on the Gherkin specification language.

### Hooks

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

#### Tag Scoping

Most hooks support tag scoping. Use tag scoping to restrict hooks to only those features or scenarios that have at least one of the tags in the tag filter (tags are combined with OR). You can specify the tag in the attribute or using scoped bindings.

### Scoped bindings

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

### Context injection

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
