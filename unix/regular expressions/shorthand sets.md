# Shorthand character sets

Shorthand | Meaning | Equivalent
- | - | -
\d | Digit | [0-9]
\w | Word character | [a-zA-Z0-9_]
\s | Whitespace | [ \t\r\n]
\D | Not digit | [^0-9]
\W | Not word character | [^a-zA-Z0-9_]
\S | Not whitespace | [^ \t\r\n]

Speaking of `\w` — underscore is a word character, hyphen is not.

Examples:

`/\d\d\d\d/` matches "1984", but not "text"

`/\w\w\w/` matches "ABC", "123", and "1_A"

`/\w\s\w\w/` matches "I am", but not "Am I"

You can also put this character sets inside character sets:

`/[\w\-]/` matches a word characer or hypen (useful)

`/[\d\s]/` matches any digit or whitespace character

`/[^\d]/` is the same as `/D/` and `/[^0-9]/`

A word of caution though about using negatives, especially when we already have negative shorhands:

`/[^\d\s]/` is not the same as `/[\D\S]/`

`/[^\d\s]/` = NOT digit OR space character

`/[\D\S]/` = EITHER NOT digit OR NOT space character
