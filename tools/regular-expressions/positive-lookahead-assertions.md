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

## Double-testing with lookahead assertions

With lookahead assertion we can match a pattern that also matches another pattern.

`/\d{3}-\d{3}-\d{4}/` matches "555-302-4321" and "555-781-6978"

`/^[0-5\-]+$/` matches "555-302-4321" and "23140-5"

We can put both of the above conditions together:

`/(?=^[0-5\-]+$)\d{3}-\d{3}-\d{4}/` matches "555-302-4321"

Lookahead assertions allow us to run multiple regular expression tests on the same string before it returns a successful match, so we can keep stacking assertions:

`/(?=^[0-5\-]+$)(?=.*4321)\d{3}-\d{3}-\d{4}/` matches "55-302-4321"

This is a powerful stuff that lets us write expressions we wouldn't be able to write otherwise.

Lookahead assertions makes clearer what our intentension is, it's easier to read and understand what we are going for, what our requirements are.

Expression that requires a password to be from 8 to 15 characters in length, to have a least one digit and one upper-case letter:

`/^(?=.*\d)(?=.*[A-Z]).{8,15}$/` matches "sword42Fish"
