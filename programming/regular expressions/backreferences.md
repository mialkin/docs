# Backreferences

[Grouped expessions](grouping.md) are captured by regex engine — as regular expression is going through and finding matches, it stores the matched portions in parentheses, it bookmarks them, remembers them for later.

For example `/a(p{2}l)e/` matches "apple" and stores "ppl". It happens automatically and by default — when regex engine sees parentheses it captures the data that is in there for later. Backreferences allow access to captured data. We can refer to the backreference with syntax "backslash followed by a number" — `\1`.

Metacharacter | Meaning
-|-
\1 through \9 | Backreferences for positions 1 to 9