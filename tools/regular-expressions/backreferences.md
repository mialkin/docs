# Backreferences

## Table of contents

- [Backreferences](#backreferences)
  - [Table of contents](#table-of-contents)
  - [Support](#support)
  - [Examples](#examples)
  - [Backreferences to optional expressions](#backreferences-to-optional-expressions)
    - [1. Element is optional, but group/capture is not optional](#1-element-is-optional-but-groupcapture-is-not-optional)
    - [2. Element is not optional, but group/capture is optional](#2-element-is-not-optional-but-groupcapture-is-optional)
  - [Find and replace using backreferences](#find-and-replace-using-backreferences)
    - [Example](#example)

[Grouped expressions](grouping.md) are captured by regex engine: as regular expression engine is going through and finding matches, it stores the matched portions in parentheses, it bookmarks them, remembers them for later.

For example `/a(p{2}l)e/` matches "apple" and stores "ppl". It happens automatically and by default: when regex engine sees parentheses it captures the data that is in there for later. Backreferences allow access to captured data. We can refer to the backreference with syntax "backslash followed by a number" — `\1`.

| Metacharacter | Meaning                             |
| ------------- | ----------------------------------- |
| \1 through \9 | Backreferences for positions 1 to 9 |

There are typically two ways that you would use those backreferences:

1. You can refer back to backreference in the same expression where you captured the group later on.

2. You can refer back to backreference after the matching is complete. To do so you would need to be inside a programming language, so that matched data could be stored in a variable and referred to those matched portions. Or another place that comes up is inside a text editor. If you are doing find and replace, than the part that is captured during the find action can be referred to while you are doing the replace action.

## Support

Most regex engines support `\1` through `\9`. Some regex engines use `$1` through `$9` instead.

Some regex engines support `\1` through `\99`.

## Examples

`/(apples) to \1/` matches "apples to apples".

`/(ab)(cd)(ef)\3\2\1/` matches "abcdefefcdab".

`/(<i|em>).+?</\1>/` matches "\<i>Hello\</i>" and "\<em>Hello\</em>", does not match "\<i>Hello\</em>" and "\<em>Hello\</i>".

## Backreferences to optional expressions

Let's take a look at two special cases when backreferences refer to optional expressions.

### 1. Element is optional, but group/capture is not optional

`/A?B/` matches "AB" and "B".

`/(A?)B/` matches "AB" and captures "A".

`/(A?)B/` matches "B" and captures "", i.e. nothing, empty string.

`/(A?)B\1/` matches "ABA" and "B".

`/(A?)B\1C/` matches "ABAC" and "BC".

### 2. Element is not optional, but group/capture is optional

`/A?B/` matches "AB" and "B".

`/(A?)B/` matches "AB" and captures "A".

`/(A?)B/` matches "B" and does not capture anything.

`/(A)?B\1/` matches "ABA" but not "B" — that's true in every regex engine except JavaScript.

## Find and replace using backreferences

In order to do find and replace we will either need a programming language or a text editor.

Steps:

1. Create a regular expression that matches target data
2. Add capturing groups
3. Write the replacement string. Use all captures; may need to use $1 instead of \1

### Example

Find: `/^(\d{1,2}),([\w .]+?)([\w]+?),(\d{4})/`.

Replace: $1,$3,$2,$4.
