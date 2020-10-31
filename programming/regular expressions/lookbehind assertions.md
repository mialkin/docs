# Lookbehind assertions

Metacharacter | Meaning
---------------|------------------------------
?<=            | Positive lookbehind assertion
?<!            | Negative lookbehind assertion

Any valid regular expression can be used inside lookbehind assetions.

## Syntax

`/(?<=regex)/`

`/(?<!regex)/`

## Examples

`/(?<=base)ball/` matches the "ball" in "baseball" but not "football". It's the same as `/ball(?<=baseball)/`

`/(?<!base)ball/` matches the "ball" in "football" but not "baseball"
