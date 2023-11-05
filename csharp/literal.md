# Literal

A **literal** is a value that is used by the variables.

## Table of contents

- [Literal](#literal)
  - [Table of contents](#table-of-contents)
  - [Literal types](#literal-types)
    - [Integer](#integer)
      - [Decimal literal](#decimal-literal)
      - [Octal literal](#octal-literal)
      - [Hexadecimal literal](#hexadecimal-literal)
      - [Binary literal](#binary-literal)
    - [Floating-point](#floating-point)
    - [Character](#character)
    - [String](#string)
    - [Null](#null)
    - [Boolean](#boolean)
  - [Links](#links)

## Literal types

### Integer

A literal of integer type is known as the integer literal. It can be decimal, octal, hexadecimal, or binary constant. No prefix is required for the decimal numbers. A suffix can also be used with the integer literals like `U` or `u` are used for unsigned numbers while `l` or `L` are used for long numbers. By default, every literal is of `int` type.

#### Decimal literal

Decimal literal, base 10:

```csharp
int x = 101; // Allowed digits are 0–9
```

#### Octal literal

Octal literal, base 8:

```csharp
// The octal number should be prefixed with 0
// Allowed digits are 0-7
const int octalNumber = 012;
Console.WriteLine(octalNumber); // 12

// Octal to decimal conversion:
int decimalRepresentation = Convert.ToInt32(value: octalNumber.ToString(), fromBase: 8);
Console.WriteLine(decimalRepresentation); // 10
```

#### Hexadecimal literal

Hexadecimal literal, base 16:

```csharp
// The hexadecimal number should be prefixed with 0X or 0x
// Allowed digits are 0-9 and characters are a-f
// Both uppercase and lowercase characters can be used
int x = 0X123FacE;
int y = 0x123Face;

Console.WriteLine(x); // 19135182
Console.WriteLine(x == y); // True
```

#### Binary literal

```csharp
int x = 0b101;
Console.WriteLine(x); // 5
```

### Floating-point

### Character

### String

### Null

### Boolean

## Links

[↑ C# Literals](https://www.geeksforgeeks.org/c-sharp-literals/).
