# Negative lookahead assertions

Negative lookahead assertions give us a way to describe expressions that should be rejected.

Metacharactes | Meaning
--------------|----------------------------
?!            | Negative lokahead assertion

## Syntax

`/(?!regex)/`

## Example

`/(?!seashore)sea/` matches "sea" in "seaside" but not "seashore". It's the same as `/sea(?!shore)/`

`/online(?! training)/` does not match "online training"

`/online(?!.*training)/` does not match "online video training"
