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

## Repeating alternations

You can repeat an alternation becuase it's just simply a [group](grouping.md), and group can be repeated:

`/(AA|BB|CC){6}/` matches "AABBAACCAABB".

## Nesting alternations

We can nest alternations as well:

`/(\d{2}([A-Z]{2}|-\d\w\d\w)|\d{4}(-\d{2}-[A-Z]{2,8}|_x[A-F]))/`

 With nesting it becomes a trade-off between precision, readbility, and efficiency.
