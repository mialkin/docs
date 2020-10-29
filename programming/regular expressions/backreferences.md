# Backreferences

[Grouped expessions](grouping.md) are captured by regex engine — as regular expression engine is going through and finding matches, it stores the matched portions in parentheses, it bookmarks them, remembers them for later.

For example `/a(p{2}l)e/` matches "apple" and stores "ppl". It happens automatically and by default — when regex engine sees parentheses it captures the data that is in there for later. Backreferences allow access to captured data. We can refer to the backreference with syntax "backslash followed by a number" — `\1`.

Metacharacter | Meaning
-|-
\1 through \9 | Backreferences for positions 1 to 9

There are typically two ways that you would use those backreferences:

1. You can refer back to backreference in the same expression where you captured the group later on.

2. You can refer back to backreference after the matching is complete. To do so you would need to be inside a programming language, so that matched data could be stored in a variable and refered to those matched portions. Or another place that comes up is inside a text editor — if you are doing find and replace, than the part that is captured during the find action can be refered to while you are doing the replace action.

## Support

Most regex engines support `\1` through `\9`. Some regex engines use `$1` through `$9` instead.

Some regex engines support `\1` through `\99`.

## Examples

