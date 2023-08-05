# Tell Don't Ask

**Tell, Don't Ask** or **TDA** is a principle that suggests that it is better to issue an object a command do perform some operation or logic, rather than to query its state and then take some action as a result.

It is related to the Flags Over Objects anti-pattern as well as the Anemic Domain Model anti-pattern. You can easily spot violations of TDA in code that queries or uses several properties of an object in order to perform some calculation. This is especially problematic when the same kind of calculation is done in many places (violating the Don't Repeat Yourself principle), but can represent a design deficiency even if it only occurs in one location in the current codebase.

## Links

[↑ Tell, Don't Ask](https://deviq.com/principles/tell-dont-ask).

[↑ TellDontAsk](https://martinfowler.com/bliki/TellDontAsk.html).
