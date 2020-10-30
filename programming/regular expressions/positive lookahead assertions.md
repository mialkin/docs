# Positive lookahead assertions

Positive lookahead assertion is an assertion of what ought to lie ahead. We are telling regular expression engine — take this grouped expression and look ahead and see if you can find a match. If our lookahed expression fails, then the whole match will fail, but if not, then the engine will keep going and see if it can make match out of everything else in the expression.

Any valid regular expression can be used inside the lookahead assertion.

Metacharacter | Meaning
-|-
?= | Positive lookahead assertion

You would use `?=` meatacharacter inside a grouped expression as the first characters inside the parentheses.

## Syntax

`/(?=regex)/`

## Examples

`/(?=seashore)sea/` matches "sea" in "seashore" but not in "seaside"
