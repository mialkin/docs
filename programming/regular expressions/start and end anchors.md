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
