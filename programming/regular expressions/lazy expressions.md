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

The `?` instructs quantifier to use a "lazy strategy" for making choices. Lazy strategy matches as little as possible before giving control to the next.

## Examples

`/\w*?\d{3}/`

`/[A-Za-z-]+?\./`

`/.{4,8}?_.{4,8}/`

`/apples??/`

`/\d+\w+?\d+/` matches **01_FY_07**_report_99.xls

`/".+?", ".+?"/` matches **"Milton", "Waddams"**, "Initech, Inc."
