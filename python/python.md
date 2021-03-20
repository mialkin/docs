# Python

- [Python](#python)
  - [Comments](#comments)
  - [Division](#division)
  - [Exponentiation](#exponentiation)
  - [Boolean values](#boolean-values)
  - [Strings](#strings)

## Comments

```python
# Single line comments

""" Multiline strings
    comment
"""
```

## Division

The result of division is always a floating-point number:

```python
10.0 / 3  # => 3.3333333333333335
1 / 1     # => 1.0
```

Integer division `//` rounds down for both positive and negative numbers:

```python
5 // 3       # => 1
-5 // 3      # => -2
5.0 // 3.0   # => 1.0 # works on floats too
-5.0 // 3.0  # => -2.0
```

## Exponentiation

```python
2**3  # => 8
```

## Boolean values

```python
True   # => True
False  # => False
```

Negation is done with `not`:

```python
not True        # => False
not False       # => True
True and False  # => False
False or True   # => True
```

`True` and `False` are actually `1` and `0`:

```python
True + True # => 2
True * 8    # => 8
False - 5   # => -5
```

Comparison operators examine numerical values of `True` and `False`:

```python
0 == False  # => True
1 == True   # => True
2 == True   # => False
-5 != False # => True
```

## Strings

```python
"This is a string"
'This is a string too'
```

String concatenation:

```python
"Hello " + "world!"  # => "Hello world!"
"Hello " "world!"    # => "Hello world!"
```

A string can be treated like a list of characters:

```python
"Hello world!"[0]  # => 'H'
```

```python

```

```python

```

```python

```
