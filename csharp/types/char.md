# `char`

The `char` type keyword is an alias for [↑ `Char`](https://learn.microsoft.com/en-us/dotnet/api/system.char) structure type that represents a [Unicode](/programming/unicode.md) UTF-16 character.

`Char` structure represents a character as a [↑ UTF-16](https://en.wikipedia.org/wiki/UTF-16) code unit. The value of a `Char` object is its 16-bit numeric, ordinal, value.

The default value of the `char` type is `\0`, that is, [↑ U+0000](https://www.compart.com/en/unicode/U+0000).

## Literals

You can specify a `char` value with:

- a character [literal](/csharp/literal.md)
- a Unicode escape sequence, which is `\u` followed by the four-symbol hexadecimal representation of a character code
- a hexadecimal escape sequence, which is `\x` followed by the hexadecimal representation of a character code

```csharp
const char cyrillic = 'Б';
const int integerCyrillic = cyrillic;
var hexadecimalCyrillic = integerCyrillic.ToString(format: "X"); // Hexadecimal value in string form

Console.WriteLine(cyrillic);            // Б
Console.WriteLine(integerCyrillic);     // 1041
Console.WriteLine(hexadecimalCyrillic); // 411

Console.WriteLine('Б');         // Б
Console.WriteLine('\u0411');    // Б
Console.WriteLine('\x411');     // Б
Console.WriteLine('\x0411');    // Б
Console.WriteLine((char)1041);  // Б
```

## Links

[↑ char (C# reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/char).
