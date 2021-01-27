# Non-capturing group expressions

Metacharacter | Meaning
-|-
?: | Specify a non-capturing group

By default, wherever we put parentheses around the expression it gets automatically captured.

Now we are going to tell regex engine to turn off that default behavior by using two this metacharacters together — `?:`.

That turn off capture and backreferences. Doing so you can optimize your regular expression for speed; you can also preserve space for more captures for those engines that are limited to 9 captures.

## Syntax

`/(\w+)/` becomes `/(?:\w+)/`

## Support

Most regex engines support non-capturing group expressions.

## Examples

`(oranges) and (apples) to \1` matches "oranges and apples to oranges"

`(oranges) and (apples) to \2` matches "oranges and apples to apples"

`(?:oranges) and (apples) to \1` matches "oranges and apples to apples"

It's important to note, that when you have this syntax `/(?:regex)/`, the `?` has the meaning "give this group a different meaning", and after that the `:` has the meaning "the meaning is not-capturing".
