# Character set

In a way a [wildcard](wildcard.md) character is a character set too — it's just a character set that matches all characters, or it's a character set of all characters.

`[` and `]` are metecharacters that indicate a character set which will match any *one* of several characters — the characters that are inside the set. The order of characters inside the set does not matter.

## Examples

`/[aeiou]/` matches any one vowel

`/gr[ea]y/` matches "grey" and "gray"

`/gr[ea]t/` does not match "great"
