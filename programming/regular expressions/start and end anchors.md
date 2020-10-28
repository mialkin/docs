# Start and end anchors

Metacharacters | Meaning
---------------|----------------------
^              | Start of string/line
$              | End of string/line
\A             | Start of string, never end of line
\Z             | End of string, never end of line

## Support

`^` and `$` metacharacters are supported in all regex engines.

`\A` and `\Z` metacharacters are supported in Java, .NET, PHP, Python, Ruby.

## Example

`/^\w+@\w+\.[a-z]{3}$/` matches "**nobody@nowhere.com**".

`/^[A-Z]/` matches "**M**r. Smith went to Wasington.".

## Line breaks and multiline mode

By default regular expressions use single-line mode and characters `^`, `$`, `\A`, and `\Z` do not match at the line breaks.

The expression `/^[a-z]+/` matches only the first line (i.e. only **milk**) of the string:

```text
milk
apple juice
sweet peas
yogurt
sweet corn
apple sauce
milkshake
sweet potatoes
```

Many Unix tools support only single line.

We can put regular expression engine in multiline mode, and after that `^` and `$` characters will start matching at the start and the end of the lines, but `\A` and `\Z` do not match at line breaks even in multiline mode.

With multiline option turned on the expression `/^[a-z]+/` matches each line of the string:

```text
milk
apple juice
sweet peas
yogurt
sweet corn
apple sauce
milkshake
sweet potatoes
```

## Languages support of multiline

Most languages offer a multiline option:

**.NET**:
  
```csharp
Regex.Match("string" , "^regex$", RegexOptions.Multiline)
```

**JavaScript**:

```js
/^regex$/m
```
