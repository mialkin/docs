# Negative character set

`^` metacharacter negates a character set — it means that not any one of characters.

We put it as the first character inside a character set, and it negates character set. `^` still represents just *one* character.

Examples:

`/[^aeiou]/` matches any one consonant (non-vowel)

`/see[^mn]/` matches "seek" and "sees" but not "seem" or "seen"

Caution:

`/see[^mn]/` matches "see " but not "see"
