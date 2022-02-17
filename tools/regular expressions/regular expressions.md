# Regular expresssions

A **regular expression** is a set of symbols representing a text pattern.

## TOC

- [Regular expresssions](#regular-expresssions)
  - [TOC](#toc)
  - [Notation](#notation)
  - [Metacharacters](#metacharacters)
  - [Other special characters](#other-special-characters)
  - [Modes](#modes)
    - [Standard vs global matching](#standard-vs-global-matching)
    - [Dot-matches-all](#dot-matches-all)
    - [Eagerness](#eagerness)

## Notation

Wherever we have a regular expression, you are going to see it inside forward slashes:

```text
/abc/
```

This notation with 2 forward slashes is a convention and it is used all the time.

## Metacharacters

A **metacharacter** is a character that has special meaning. Metacharacters transform literal characters into powerful expressions:

```text
\ . * + - {} [] ^ $ | ? () : ! =
```

Metacharacters can have more than one meaning which depends on how they are used in context. Metacharacters have variations between different regex engines.

List of metacharacters:

* [. wildcard](wildcard.md)
* [\ escaping](escaping.md)
* [[] character set](set.md)
  * [- character range](range.md)
  * [^ negative character set](negative%20set.md)
  * [\d \w \s shorthand character sets](shorthand%20sets.md)
* [*, +, ? repetition metacharacters](repetition.md)
* [{min,max} quantified repetition](quantified%20repetition.md)
* [Greedy expressions](greedy%20expressions.md)
* [*?, +?, {min,max}?, ?? lazy expressions](lazy%20expressions.md)
* [() grouping metacharacters](grouping.md)
* [| alternation metacharacter](alternation.md)
* [^, $ start and end anchors](start%20and%20end%20anchors.md)
* [\b, \B word boundaries](word%20boundaries.md)
* [\1 through \9 backreferences](backreferences.md)
* [?: non-capturing group expressions](non-capturing.md)
* Lookaround assertions
  * [?= positive lookahead assertions](positive%20lookahead%20assertions.md)
  * [?! negative lookahead assertions](negative%20lookahead%20assertions.md)
  * [?<=, ?<! positive and negative lookbehind assertions](lookbehind%20assertions.md)

## Other special characters

* Spaces
* Tabs `\t`
* Line returns `\r`, `\n`, `\r\n`
* Non-printable characters
  * bell `\a`, escape `\e`, form feed `\f`, vertical tab `\v`
* ASCII or ANSI codes
  * Codes that control appearance of a text terminal
  * 0xA9 = \xA9

The expression `\e` will match letter `e` in the word "r**e**staurant".

Similary the expression `\a` will match as well: "rest**a**ur**a**nt".

On the other hand expressions `\n` and `\t` will not match because they have another meaning — new line and tab accordingly.

> Putting the backslash in front of a character *does* make character look for something different.

## Modes

Modes go right after slashes, they are not a part of regullar expressions, they are modifiers for the way this regular expressions ought to be handled:

* Standard: `/re/`
* Global: `/re/g`
* Case-insensitive: `/re/i`
* Multiline: `/re/m`
* Dot-matches-all: `/re/s`

### Standard vs global matching

Standard (non-global) matching: earliest (leftmost) match is always preffered:

`/zz/` matches the first set of z's in "pi**zz**azz".

Global matching: all matches are found throughout the text:

`/zz/g` matches the both sets of z's in "pi**zz**a**zz**".

### Dot-matches-all

With dot-matches-all on, the following text will be a match for a regular expression `/h.t/`:

```text
h
t
```

### Eagerness

Regular expressions are eager — they are eager to return a match to you.
