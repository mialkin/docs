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

`/(?=seashore)sea/` matches "sea" in "seashore" but n ot in "seaside"

You can write the same expression this way as well:

`/sea(?=shore)/` — what we are essentially saying here is — if you find a match for "sea", now look ahead at what comes next  and see if you have a match for "shore", see if that's what follows.

Both expressions from the above will do the same — they will just match "sea" and only when it's inside a "seashore", not "seaside" for example.

`/sea/` matches "**sea**shore **sea**side"

`/(?=seashore)sea/` matches "**sea**shore seaside"

`/sea(?=shore)/` matches "**sea**shore seaside"

On a constrast using [non-capturing expression](non-capturing.md) — it's not the same:

`/sea(?:shore)/` matches "**seashore** seaside"

In the above we are talking about what *gets matched*, not what *gets captured*

Here we want to find all words that are followed by a comma:

`/\b[A-Za-z']\b+(?=,)/`

So it will match words only, not comma.
