# Modes

Modes go right after slashes, they are not a part of regullar expressions, they are modifiers for the way this regular expressions odd to be handled:

* Standard: `/re/`
* Global: `/re/g`
* Case-insensitive: `/re/i`
* Multiline: `/re/m`
* Dot-matches-all: `/re/s`

## Standard vs global matching

Standard (non-global) matching: earliest (leftmost) match is always preffered:

`/zz/` matches the first set of z's in "pi**zz**azz".

Global matching: all matches are found throughout the text:

`/zz/g` matches the both sets of z's in "pi**zz**a**zz**".

## Eagerness

Regular expressions are eager — they are eager to return a match to you.
