# Regular expresssions

A **regular expression** is a set of symbols representing a text pattern.

Composition of acronym `grep`:

```text
g/Regular Expression/p
```

`g` — search globally, `p` — output results, i.e. "print", so it makes it `G`lobal `R`egular `E`xpression `P`rint.

## Modes

* Standard: `/re/`
* Global: `/re/g`
* Case-insensitive: `/re/i`
* Multiline: `/re/m`
* Dot-matches-all: `/re/s`

Modes go right after slashes, they are not a part of regullar expressions, they are modifiers for the way this regular expressions odd to be handled.
