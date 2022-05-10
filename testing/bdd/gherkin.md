# Gherkin

**Gherkin** is a language for writing behavior scenarios.

Gherkin's code is written into *feature files* which have `.feature` extension.

The official Gherkin language standard is maintained by [↑ Cucumber](https://cucumber.io), one of the most prevalent BDD automation frameworks. Most other BDD frameworks use Gherkin, but some may not conform 100% to Cucumber's language standards.

Gherkin uses a set of special *keywords* to give structure and meaning to executable specifications.

Either spaces or tabs may be used for indentation. The recommended indentation level is two spaces.

## Table of Contents

- [Gherkin](#gherkin)
  - [Table of Contents](#table-of-contents)
  - [Feature](#feature)
  - [Scenario](#scenario)
  - [Step](#step)
  - [Tag](#tag)
  - [Descriptions](#descriptions)
  - [Asterisk](#asterisk)
  - [Localization](#localization)

## Feature

A **feature** is an abstraction that helps to group related *scenarios*.

The first primary keyword in a Gherkin document must always be the `Feature`, followed by a `:` and a short text that describes the feature.

```gherkin
Feature: Guess the word

  The word guess game is a turn-based game for two players.
  The Maker makes a word for the Breaker to guess. The game
  is over when the Breaker guesses the Maker's word.

  Scenario: Maker starts a game
```

## Scenario

A **scenario** is a concrete example that illustrates a business rule. It consists of one or more *steps*.

The keyword `Scenario` is a synonym of the keyword `Example`.

## Step

A **step** is a way to map a Gherkin sentence in the specification to a method in the code.

Each step starts with one of the following keywords: `Given`, `When`, `Then`, `And`, `But`.

A BDD framework executes each step in a scenario one at a time, in the sequence they have been written them in. When a framework tries to execute a step, it looks for a matching step definition to execute.

Keywords are not taken into account when looking for a step definition. This means you cannot have a `Given`, `When`, `Then`, `And` or `But` step with the same text as another step.

## Tag

A **tag** is a marker that can be assigned to a feature or a scenario. Assigning a tag to a feature is equivalent to assigning the tag to all scenarios in the feature file.

You can use tags to filter and group generated from specification test methods. For example, you can tag crucial tests with `@important` tag, and then execute these tests more frequently.

## Descriptions

Free-form descriptions can be placed underneath `Feature`, `Example`, `Scenario`, `Background`, `Scenario Outline` and `Rule`.

You can write anything you like, as long as no line starts with a keyword.

## Asterisk

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

## Localization

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