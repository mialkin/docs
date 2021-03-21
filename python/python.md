# Python

- [Python](#python)
  - [Comments](#comments)
  - [Division](#division)
  - [Exponentiation](#exponentiation)
  - [`is` vs `==`](#is-vs-)
  - [Booleans](#booleans)
  - [Strings](#strings)
  - [Environment variables](#environment-variables)
  - [Lists](#lists)

## Comments

```py
# Single line comments

""" Multiline strings
    comment
"""
```

## Division

The result of division is always a floating-point number:

```py
10.0 / 3  # => 3.3333333333333335
1 / 1     # => 1.0
```

Integer division `//` rounds down for both positive and negative numbers:

```py
5 // 3       # => 1
-5 // 3      # => -2
5.0 // 3.0   # => 1.0 # works on floats too
-5.0 // 3.0  # => -2.0
```

## Exponentiation

```py
2**3  # => 8
```

## `is` vs `==`

`is` checks if two variables refer to the same object, whereas `==` checks
if the pointed objects have the same values:

```py
a = [1, 2, 3, 4]  # Point a at a new list, [1, 2, 3, 4]
b = a             # Point b at what a is pointing to
b is a            # => True, a and b refer to the same object
b == a            # => True, a's and b's objects are equal
b = [1, 2, 3, 4]  # Point b at a new list, [1, 2, 3, 4]
b is a            # => False, a and b do not refer to the same object
b == a            # => True, a's and b's objects are equal
```

## Booleans

```py
True   # => True
False  # => False
```

Negation is done with `not`:

```py
not True        # => False
not False       # => True
True and False  # => False
False or True   # => True
```

`True` and `False` are actually `1` and `0`:

```py
True + True # => 2
True * 8    # => 8
False - 5   # => -5
```

Comparison operators examine numerical values of `True` and `False`:

```py
0 == False  # => True
1 == True   # => True
2 == True   # => False
-5 != False # => True
```

## Strings

```py
"This is a string"
'This is a string too'
```

Length:

```py
len("summer")   # => 6
```

Concatenation:

```py
"Hello " + "world!"  # => "Hello world!"
"Hello " "world!"    # => "Hello world!"
```

Interpolation:

```py
name = "Aleksei"
f"My name is {name}"                       # => "My name is Aleksei"
f"{name} is {len(name)} characters long"   # => "Aleksei is 7 characters long"
```

A string can be treated like a list of characters:

```py
"Hello world!"[0]  # => 'H'
```

## Environment variables

```py
import os
    print(os.environ['HOME'])
```


## Lists

```py
my_list = [4, 5, 6] # Quick list initialization

li = []
li.append(1)    # Append new element

li[0]       # Get element by index
li[-1]      # Get last element
li.pop()    # Remove last element
```

Ranges:

```py
li[1:3]   # Return list from index 1 to 3
li[2:]    # Return list starting from index 2
li[:3]    # Return list from beginning until index 3
li[::2]   # Return list selecting every second entry
li[::-1]  # Return list in reverse order => [3, 4, 2, 1]
```

```py

```
