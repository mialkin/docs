# Metacharacters

A **metacharacter** is a character that has special meaning. Metacharacters transform literal characters into powerful expressions:

```text
\ . * + - {} [] ^ $ | ? () : ! =
```

Metacharacters can have more than one meaning which depends on how they are used in context.

Metacharacters have variations between different regex engines.

## Wildcard

Metacharacter `.` matches any character except newline. Original Unix regex tools were line-based, that's why we have to deal with new lines separately.

`/h.t/` matches "h**a**t", "h**o**t", and "h**i**t", but not "heat".

The challange of regular expression is both matching what you want and *only* what you want.
