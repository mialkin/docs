# Character set

In a way a [wildcard](wildcard.md) character is a character set too — it's just a character set that matches all characters, or it's a character set of all characters.

`[` and `]` are metecharacters that indicate a character set which will match any *one* of several characters — the characters that are inside the set. The order of characters inside the set does not matter.

## Examples

`/[aeiou]/` matches any one vowel

`/gr[ea]y/` matches "grey" and "gray"

`/gr[ea]t/` does not match "great"

## Metacharacters inside characrter set

Metacharacters inside character sets are already escaped, you do not need to escape them again:

`h[abc.xyz]t` matches "hat" and "h.t", but not "hot"

There are some exceptions for this and they make sence: the metacharacters that have to do with character sets: `] - ^ \` — this characters do need to be escaped. You may not always be required to escape them though — each regex engine handles them differently.
 