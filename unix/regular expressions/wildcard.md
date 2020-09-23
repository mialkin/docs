# Wildcard metacharacter

`.` metacharacter matches any character except newline. Original Unix regex tools were line-based, that's why we have to deal with new lines separately.

`/h.t/` matches "h**a**t", "h**o**t", and "h**i**t", but not "heat".

The challange of regular expression is both matching what you want and *only* what you want.

With dot-matches-all [mode](modes.md) on, the following text will be a match for a regular expression `/h.t/`:

```text
h
t
```
