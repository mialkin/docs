# Grouping metacharacters

Meatacharacter | Meaning
-|-
( | Start grouped expression
) | End grouped expression

We put parentheses around things that we want to group together. Putting this metacharacters around either a couple of different characters or couple of different [character sets](set.md) allow us applying repetition to that group.

Grouping metacharacters make reading regular expression easier to read in some cases.

Grouping meracharacters capture the group for use in matching and replacing.

Grouping meracharacters cannot be used inside of a character set — there is no reason to group anything inside a character set. The purpose of a character set is to define a set of characters. We can put groups around character sets, we can group them around several character sets, but not inside — inside they have they literal meaning.

## Examples

`/(abc)+/` matches "abc" and "abcabcabc"

`/(in)?dependent/` matches "independent" and "dependent"

`/run(s)?/` is the same as `/runs?/`
