# Escaping metacharacters

`\` metacharacter escapes the next character; it allows use of metacharacters as literal characters.

Match a period with `\.`:
`/9\.00/` matches "9.00", but not "9500" or "9-00".

Match a backslash by escaping a backslash: `\\`.

Escaping should be used only for metacharacters. Literal characters should never be escaped: it gives them meaning.

Quotation marks are not metacharacters, do not need to be escaped.

You may need to also escape forward slashes:

```text
/home/user/document.txt
```

If you would want to write something that would match previous text you can write something like this:

```text
/home/user/document\.txt
```

In this case you are not typing forward slashes at the beginning and at the end of regular expression. If forward slashes are needed to be there you'll need to escape slashes in regular expression:

 ```text
 /\/home\/user\/document\.txt/
 ```

It says to regex engine that we are not done with our regular expression yet: remember that the second forward slash is the indicator that our regular expression is all done.
