# Regular expresssions

A **regular expression** is a set of symbols representing a text pattern.

Composition of acronym `grep`:

```text
g/Regular Expression/p
```

`g` — search globally, `p` — output results, i.e. "print", so it makes it `G`lobal `R`egular `E`xpression `P`rint.

## Notation

Wherever we have a regular expression, you are going to see it inside of forward slashes:

```text
/abc/
```

This notation with 2 forward slashes is a notation and is used all the time. This convention comes back to `ed` editor where regular expression search was performed as:

```text
g/re/p
```

Ken Thompson implemented in 1968 regular expressions in `ed`, an early Unix text editor.

## Modes

* Standard: `/re/`
* Global: `/re/g`
* Case-insensitive: `/re/i`
* Multiline: `/re/m`
* Dot-matches-all: `/re/s`

Modes go right after slashes, they are not a part of regullar expressions, they are modifiers for the way this regular expressions odd to be handled.
