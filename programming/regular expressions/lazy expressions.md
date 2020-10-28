# Lazy expressions

Metacharacter | Meaning
-|-
? | Make preceding quantifier lazy

## Syntax

* `*?`

* `+?`

* `{min,max}?`

* `??`

## Greedines

Regular expression engines are eager. 

Greedines makes regular expression engines match as much characters as possible before giving control to the next expression part:

`/peanut(butter)?/` will match **peanutbutter**.

Regular expression engines are lazy:

`/(peanut|peanutbutter)/` will match **peanut**butter, not **peanutbutter**.

## Lazy strategy

The `?` instructs quantifier to use a "lazy strategy" for making choices:

`/\w*?\d{3}/`

`/[A-Za-z-]+?\./`

`/.{4,8}?_.{4,8}/`

`/apples??/`

`/\d+\w+?\d+/` matches **01_FY_07**_report_99.xls

`/".+?", ".+?"/` matches **"Milton", "Waddams"**, "Initech, Inc."
