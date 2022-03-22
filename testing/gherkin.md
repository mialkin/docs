# Gherkin

**Gherkin** is a language for writing behavior scenarios.

Gherkin's code is written into *feature files* which have `.feature` extension.

The official Gherkin language standard is maintained by [↑ Cucumber](https://cucumber.io), one of the most prevalent BDD automation frameworks. Most other BDD frameworks use Gherkin, but some may not conform 100% to Cucumber's language standards.

Gherkin uses a set of special keywords to give structure and meaning to executable specifications. Most lines in a Gherkin document start with one of the keywords.

Either spaces or tabs may be used for indentation. The recommended indentation level is two spaces.

## Table of Contents

- [Gherkin](#gherkin)
  - [Table of Contents](#table-of-contents)
  - [Keywords](#keywords)
    - [`*`](#)
  - [Localization](#localization)

## Keywords

### `*`

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
| `ScenarioOutline` | `Структура сценария`, `Шаблон сценария`                 |
| `Examples`        | `Примеры`                                               |
| `Given`           | `*`, `Допустим`, `Дано`, `Пусть`                        |
| `When`            | `*`, `Когда`, `Если`                                    |
| `Then`            | `*`, `То`, `Затем`, `Тогда`                             |
| `And`             | `*`, `И`, `К тому же`, `Также`                          |
| `But`             | `*`, `Но`, `А`, `Иначе`                                 |
| `Rule`            | `Правило`                                               |
