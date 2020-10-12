# Repetition metacharacters

Metacharacter | Meaning
-|-
* | Preceding item zero or more times
+ | Preceding item one or more times
? | Preceding item zero or one time

## Examples

`/apples*/` matches "apple", "apples", and "applessssss"

`/apples+/` matches "apples" and "applessssss", but not "apple"

`/apples?/` matches "apple", "apples", but not "applessssss"

`/\d\d\d\d*/` matches numbers with three digits or more

`/\d\d\d+/` matches numbers with three digits or more

`/colou?r/` matches "color" and "colour"
