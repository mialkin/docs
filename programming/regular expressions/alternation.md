# Alternation metacharacter

Metacharacter | Meaning
-|-
\| | Match previous or next expression

The `|` metacharacter is an `OR` operator — it means either match expression on the left or match expression on the right.

The `|` metacharacter is ordered — leftmost expression gets precendece.

You can daisy-chain alternation metacharacters together.

You can also use [grouping metacharacters](grouping.md) to group our alternations to keep them distinct from the rest of our expression.

## Examples

`/apple|orange/` matches "apple" and "orange"

`/abc|def|ghi|jkl/` matches "abc", "def", "ghi", and "jkl"

`/apple(juice|sauce)/` matches "applejuice" and "applesauce"

`/w(ei|ie)rd/` matches "weird" and "wierd"
