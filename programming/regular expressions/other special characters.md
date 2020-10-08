# Other special characters

* Spaces
* Tabs (\t)
* Line returns (\r, \n, \r\n)
* Non-printable characters
  * bell (\a), escape (\e), form feed (\f), vertical tab (\v)
* ASCII or ANSI codes
  * Codes that control appearance of a text terminal
  * 0xA9 = \xA9

The expression `\e` will match letter `e` in the word "r**e**staurant".

Similary the expression `\a` will match as well: "rest**a**ur**a**nt".

On the other hand expressions `\n` and `\t` will not match because they have another meaning — new line and tab accordingly.

---

NOTE

Putting the backslash in front of a character *does* make character look for something different.

---
