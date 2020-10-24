# Regular expresssions

A **regular expression** is a set of symbols representing a text pattern.

Composition of acronym `grep`:

```text
g/Regular Expression/p
```

`g` — search globally, `p` — output results, i.e. "print", so it makes it `G`lobal `R`egular `E`xpressi/ on `P`rint.

## Metacharacters

A **metacharacter** is a character that has special meaning. Metacharacters transform literal characters into powerful expressions:

```text
\ . * + - {} [] ^ $ | ? () : ! =
```

Metacharacters can have more than one meaning which depends on how they are used in context. Metacharacters have variations between different regex engines.

## Table of content

* [Notation](notation.md)
* [Modes](modes.md)
* [Other special characters](other%20special%20characters.md)
* Metacharacters
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
  * [( ) grouping metacharacters](grouping.md)
  * [| alternation metacharacter](alternation.md)
  * [^ $ start and end anchors](start%20and%20end%20anchors.md)
