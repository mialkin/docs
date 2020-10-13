# Lazy expressions

Metacharacter | Meaning
-|-
? | Make preceding quantifier lazy

## Syntax

* `*?`

* `+?`

* `{min,max}?`

* `??`

Greedy strategy matches as much characters as possible before giving control to the next expression part.

The `?` instructs quantifier to use a "lazy strategy" for making choices. Lazy strategy matches as little as possible before giving control to the next 