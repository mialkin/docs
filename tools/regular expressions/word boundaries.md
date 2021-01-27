# Word boundaries

Metacharacter | Meaning
-|-
\b | Word boundary (start/end of word)
\B | Not a word boundary

Everything that is not a word characters, i.e. not [A-Za-z0-9_], is a subject of a new boundary.

Most regex engines support word boundaries.

## Examples

`/\b\w+\b/` finds four matches in "This is a test."

`/\b\w+\b/` matches all of "abc_123" but only parts of "top-notch", i.e. "**top**" and "**notch**".

`/\b\w+s\b/` matches "We picked **apples**."

We can also have a not a boundary, but those are not neccessarily as useful as boundary one:

`/\BThis/` does not match "This is a test." because this string begins with boundary.

`/\B\w+\B/` finds two matches in "This is a test." ("**hi**" and "**es**").
