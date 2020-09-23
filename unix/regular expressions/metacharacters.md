# Metacharacters

A **metacharacter** is a character that has special meaning. Metacharacters transform literal characters into powerful expressions:

```text
\ . * + - {} [] ^ $ | ? () : ! =
```

Metacharacters can have more than one meaning which depends on how they are used in context.

Metacharacters have variations between different regex engines.

## Wildcard

`.` metacharacter matches any character except newline. Original Unix regex tools were line-based, that's why we have to deal with new lines separately.

`/h.t/` matches "h**a**t", "h**o**t", and "h**i**t", but not "heat".

The challange of regular expression is both matching what you want and *only* what you want.

With dot-matches-all [mode](modes.md) on, the following text will be a match for a regular expression `/h.t/`:

```text
h
t
```

## Escaping metacharacters

`\` metacharacter escapes the next character; it allows use of metacharacters as literal characters.

Match a period with `\.`:

`/9\.00/` matches "9.00", but not "9500" or "9-00"

Escaping should be used only for metacharacters. Literal characters should never be escaped — it gives them meaning.

Quotation marks are not metacharacters, they do not need to be escaped.

You may need to also escape forward slashes:

```text
/home/user/document.txt
```

If you would want to write something that would match previous text you can write something like this `/home/user/document.txt`. In this case you are not typing forward slashes at the beggining and at the end of regular expression. If forward slashes are needed to be there you'll need to escape slashes in regular expression: `/\/home\/user\/document\.txt/`. It says to regex engine that we are not done with our regular expression yet: remember that the second forward slash is the indicator that our regular expression is all done.
