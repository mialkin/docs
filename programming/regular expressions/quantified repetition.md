# Quantified repetition

Metacharacter | Meaning
-|-
{ | Start quantified repetition of preceding item
} | End quantified repetition of preceding item

## {min,max}

`min` and `max` are positive numbers

`min` must always be inlcuded, can be zero

`max` is optional

## Three syntaxes

`\d{4,8}` matches numbers with four to eight digits
  
`\d{4}` matches numbers with exactly four digits (min is max)

`\d{4,}` matches numbers with four or more digits (max is infinite)

## Examples

`\d{0,}` is the same as `\d*`

`\d{1,}` is the same as `\d+`

`/\d{3}-\d{3}-\d{4}/` matches most U.S. phone numbers

`/A{1,2} bonds/` matches "A bonds" and "AA bonds", not "AAA bonds"
