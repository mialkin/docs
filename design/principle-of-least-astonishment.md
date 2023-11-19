# Principle of least astonishment

The **Principle of least astonishment**, **POLA**, aka the **Principle of least surprise**, emphasizes the importance of designing software systems in a way that minimizes confusion and unexpected behavior for users and developers.

## Table of contents

- [Principle of least astonishment](#principle-of-least-astonishment)
  - [Table of contents](#table-of-contents)
  - [Rationale](#rationale)
  - [Applications](#applications)
  - [Benefits](#benefits)
  - [Examples](#examples)
    - [Naming functions](#naming-functions)
  - [Links](#links)

## Rationale

The rationale behind the Principle of least astonishment is to reduce the cognitive load on users and developers by ensuring that software behaves in a manner consistent with their expectations. By minimizing surprises and confusion, developers can more easily understand the code and users can more effectively interact with the software, leading to a better overall experience.

## Applications

The Principle of least astonishment can be applied in various aspects of software design, including:

- **UI design**: design user interfaces that are consistent, both within the application and with common platform conventions, to ensure that users can quickly learn and understand how to use the software
- **API design**: design APIs that are consistent and predictable, making it easier for developers to understand and use them correctly
- **Code structure and organization**: organize code in a logical and consistent manner, making it easier for developers to navigate and understand the codebase
- **Error handling and messaging**: provide clear and informative error messages that help users understand what went wrong and how to resolve the issue

## Benefits

Adhering to the Principle of least astonishment can lead to several benefits in software design:

- **Improved usability**: designing software that is consistent and predictable makes it easier for users to learn and use the application effectively
- **Reduced development time**: by making the codebase more intuitive and easier to understand, developers can more quickly navigate and work with the code
- **Enhanced maintainability**: a well-organized and predictable codebase is easier to maintain and extend, leading to reduced technical debt and maintenance costs
- **Increased user satisfaction**: users are more likely to have a positive experience when interacting with software that behaves in a manner consistent with their expectations

## Examples

### Naming functions

The name of a function should reflect what it does. Otherwise, a user of the function will be unpleasantly surprised:

Bad:

```c
int multiply(int a, int b)
{
    return a + b;
}
```

Better:

```c
int add(int a, int b)
{
    return a + b;
}
```

Also bad:

```c
int write_to_file(const char* filename, const char* text)
{
    printf("%s\n", text);   /* Note that filename is unused */
}
```

An example of a function that does not follow this principle is the standard C++ `remove()` function template. When applied to a collection, someone who has not studied the documentation might expect it to "remove", that is, delete, things from the collection. But it doesn't: `remove()` and `remove_if()` instead reorder elements so that the "unremoved" elements are at the head of the collection, and the size of the collection does not change. It makes sense if you read the docs, but it can be astonishing to those who haven't.

## Links

[↑ Principle of Least Astonishment: A Software Design Guideline](https://deviq.com/principles/principle-of-least-astonishment).
