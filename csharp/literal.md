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

> You can use the lowercase letter `l` as a suffix. However, this generates a compiler warning because the letter `l` can be confused with the digit 1. Use `L` for clarity

Unsigned integer, `unit`, literal:

```csharp
unit x = 304U;
Console.WriteLine(x); // 304
```

Long, `long`, literal:

```csharp
long x = 3078L;
Console.WriteLine(x); // 3078
```

Unsigned long, `ulong`, literal:

```csharp
ulong x = 965UL;
Console.WriteLine(x); // 965
```

[↑ Integral numeric types (C# reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/integral-numeric-types).

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

// Octal to decimal conversion
int decimalRepresentation = Convert.ToInt32(value: octalNumber.ToString(), fromBase: 8);
Console.WriteLine(decimalRepresentation); // 10
```

#### Hexadecimal literal

Hexadecimal literal, base 16:

```csharp
// The hexadecimal number should be prefixed with 0X or 0x
// Allowed digits are 0-9 and characters are a-f
// Both uppercase and lowercase characters can be used
int x = 0x123Face;
int y = 0X123FacE;

Console.WriteLine(x); // 19135182
Console.WriteLine(x == y); // True
```

#### Binary literal

```csharp
// The binary number should be prefixed with 0b or 0B
int x = 0b101;
int y = 0B101;
Console.WriteLine(x); // 5
Console.WriteLine(x == y); // True
```

### Floating-point

Floating-point literal is a literal that has an integer part, a decimal point, a fractional part, and an exponent part. These can be represented either in decimal form or exponential form.

```csharp
double x = 3.14145;
double y = 314145E-5;
double z = 784f;

Console.WriteLine(x); // 3,14145
Console.WriteLine(y); // 3,14145
Console.WriteLine(z); // 784
```

By default, every floating-point literal is of double type and hence we can’t assign directly to float variable. But we can specify floating-point literal as float type by suffixed with `f` or `F`.

```csharp
// float f = 3.14145;   // Won't work
float f = 3.14145F;     // Works
```

### Character

For character data types we can specify literals in 3 ways:

Single quote:

```csharp
char x = 'a';
```

Unicode representation:

```csharp
char x = '\u0061';
Console.WriteLine(x); // a
```

Escape sequence:

```csharp
char x = '\n'; // New line
char x = '\v'; // Vertical bar
```

### String

```csharp
string s1 = "Hello";
string s2 = @"Hello!";
string s3 = @"Hello";

Console.WriteLine(ReferenceEquals(s1, s2)); // False
Console.WriteLine(ReferenceEquals(s2, s3)); // False
Console.WriteLine(ReferenceEquals(s1, s3)); // True
```

### Null

The `null` keyword is a literal that represents a null reference, one that does not refer to any object. `null` is the default value of reference-type variables.

```csharp
int? x = null;
```

### Boolean

```csharp
bool x = true;
bool y = false;
```

## Links

[↑ C# Literals](https://www.geeksforgeeks.org/c-sharp-literals/).
