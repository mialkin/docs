# Enumeration types

An [↑ enumeration type](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/enum) or **enum type** is a [value type](value-types.md) defined by a set of named constants of the underlying integral numeric type.

To define an enumeration type, use the `enum` keyword and specify the names of enum members:

```csharp
enum Season
{
    Spring,
    Summer,
    Autumn,
    Winter
}
```

By default, the associated constant values of enum members are of type `int`. They start with zero and increase by one following the definition text order. You can also explicitly specify the associated constant values, as the following example shows:

```csharp
enum ErrorCode : ushort
{
    None = 0,
    Unknown = 1,
    ConnectionLost = 100,
    OutlierReading = 200
}
```

You cannot define a method inside the definition of an enumeration type. To add functionality to an enumeration type, create an extension method.

The default value of an enumeration type `E` is the value produced by expression `(E)0`, even if zero doesn't have the corresponding enum member.

## `[Flags]`

You use an enumeration type to represent a choice from a set of mutually exclusive values or a combination of choices. To represent a combination of choices, define an enumeration type as bit flags with [↑ `[Flags]`](https://learn.microsoft.com/en-us/dotnet/api/system.flagsattribute) attribute:

```csharp
const DaysOfWeek meetingDays = DaysOfWeek.Monday | DaysOfWeek.Wednesday | DaysOfWeek.Friday;

Console.WriteLine(meetingDays);

[Flags]
public enum DaysOfWeek
{
    None = 0,
    Monday = 1,
    Tuesday = 2,
    Wednesday = 4,
    Thursday = 8,
    Friday = 16,
    Saturday = 32,
    Sunday = 64

// Output:
// Monday, Wednesday, Friday

// If you don't use [Flags] attribute the output will be 21 and compiler
// will give warning `Bitwise operation on enum is not marked by [Flags] attribute`
```
