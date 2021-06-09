# String

## String as a collection of `Char`s

.NET uses the `Char` structure to represent Unicode code points by using UTF-16 encoding. The value of a `Char` object is its 16-bit numeric (ordinal) value.

A `String` object is a sequential collection of `Char` structures that represents a string of text. Most Unicode characters can be represented by a single `Char` object, but a character that is encoded as a base character, surrogate pair, and/or combining character sequence is represented by multiple `Char` objects. For this reason, a `Char` structure in a `String` object is not necessarily equivalent to a single Unicode character.

Because a single character can be represented by multiple `Char` objects, it is not always meaningful to work with individual `Char` objects. For instance, the following example converts the Unicode code points that represent the [↑ Aegean numbers](https://en.wikipedia.org/wiki/Aegean_numerals) zero through 9 to UTF-16 encoded code units. Because it erroneously equates `Char` objects with characters, it inaccurately reports that the resulting string has 20 characters.

You can use the `StringInfo` class to work with text elements instead of individual `Char` objects. The following example uses the `StringInfo` object to count the number of text elements in a string that consists of the Aegean numbers zero through nine. Because it considers a surrogate pair a single character, it correctly reports that the string contains ten characters.

```csharp
string result = String.Empty;
for (int ctr = 0x10107; ctr <= 0x10110; ctr++)  // Part of the range of Aegean numbers.
    result += Char.ConvertFromUtf32(ctr);

Console.WriteLine(result); // 𐄇𐄈𐄉𐄊𐄋𐄌𐄍𐄎𐄏𐄐
Console.WriteLine(result.Length); // 20

StringInfo si = new (result);
Console.WriteLine(si.LengthInTextElements); // 10 characters
```

[↑ Character encoding in .NET](https://docs.microsoft.com/en-us/dotnet/standard/base-types/character-encoding-introduction)

[↑ Char Struct](https://docs.microsoft.com/en-us/dotnet/api/system.char)

## Passing string to a method

```csharp
string hello = "hello";
string world = "world";

Act(hello, ref world);

void Act(string h, ref string w)
{
    h += "!";
    w += "!";
    Console.WriteLine( $"{h} {w}"); // hello! world!
}

Console.WriteLine( $"{hello} {world}"); // hello world!
```
