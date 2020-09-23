# Character range

`-` metacharacter that indicates a range of characters. It represents all characters between two characters.

Dash is a metacharacter only inside a character set, a literal dash otherwise.

Examples:

```text
[0-9]
[A-Za-z]
[a-ek-ou-y] — 15 characters
```

Caution

```text
[50-99] is not all numbers from 50 to 99, it is the same as [0-9], we are looking at text here
```
