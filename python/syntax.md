# Python syntax

- [Python syntax](#python-syntax)
  - [Comments](#comments)
  - [Division](#division)
  - [Comparison](#comparison)
  - [Exponentiation](#exponentiation)
  - [`is` vs `==`](#is-vs-)
  - [Booleans](#booleans)
  - [Strings](#strings)
  - [Environment variables](#environment-variables)
  - [Lists](#lists)
  - [Tuples](#tuples)
  - [Dictionaries](#dictionaries)
  - [None](#none)
  - [Print](#print)
  - [User input](#user-input)
  - [Ternary operator](#ternary-operator)
  - [Sets](#sets)
  - [Control flow](#control-flow)

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

## Comparison

Chaining makes this look nicer:

```py
1 < 2 < 3  # => True
2 < 3 < 2  # => False
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

`None`, `0`, and empty strings/lists/dicts/tuples all evaluate to `False`. All other values are `True`

```py
bool(0)   # => False
bool("")  # => False
bool([])  # => False
bool({})  # => False
bool(())  # => False
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

li[0]           # Get element by index
li[-1]          # Get last element
li.pop()        # Remove last element
del li[2]       # Remove element at index
li.remove(2)    # Remove first occurence of element 2
li.insert(1, 2) # Insert 2 at index 1
li.index(2)     # Get index of the first element 2
li + li2        # Concatenate li with li2 => [1, 2, 3, 4, 5, 6]
li.extend(li2)  # Concatenate li with li2
1 in li         # Check if element exists in list => True
len(li)         # Get length of list
```

Ranges:

```py
li[1:3]   # Return list from index 1 to 3
li[2:]    # Return list starting from index 2
li[:3]    # Return list from beginning until index 3
li[::2]   # Return list selecting every second entry
li[::-1]  # Return list in reverse order => [3, 4, 2, 1]
```

## Tuples

Tuples are like lists but are immutable:

```py
tup = (1, 2, 3)
tup[0]      # => 1
tup[0] = 3  # Raises a TypeError
```

Note that a tuple of length one has to have a comma after the last element but tuples of other lengths, even zero, do not:

```py
type((1))   # => <class 'int'>
type((1,))  # => <class 'tuple'>
type(())    # => <class 'tuple'>
```

You can do most of the list operations on tuples too:

```py
len(tup)         # => 3
tup + (4, 5, 6)  # => (1, 2, 3, 4, 5, 6)
tup[:2]          # => (1, 2)
2 in tup         # => True
```

You can unpack tuples (or lists) into variables:

```py
a, b, c = (1, 2, 3)  # a is now 1, b is now 2 and c is now 3
```

You can also do extended unpacking:

```py
a, *b, c = (1, 2, 3, 4)  # a is now 1, b is now [2, 3] and c is now 4
```

Tuples are created by default if you leave out the parentheses:

```py
d, e, f = 4, 5, 6  # tuple 4, 5, 6 is unpacked into variables d, e and f
```

Respectively such that d = 4, e = 5 and f = 6. Now look how easy it is to swap two values:

```py
e, d = d, e  # d is now 5 and e is now 4
```

## Dictionaries

```py
empty_dict = {}
```

Here is a prefilled dictionary:

```py
filled_dict = {"one": 1, "two": 2, "three": 3}
```

Note keys for dictionaries have to be immutable types. This is to ensure that the key can be converted to a constant hash value for quick look-ups.

Immutable types include ints, floats, strings, tuples:

```py
invalid_dict = {[1,2,3]: "123"}  # => Raises a TypeError: unhashable type: 'list'
valid_dict = {(1,2,3):[1,2,3]}   # Values can be of any type, however.
```

Look up values with `[]`:

```py
filled_dict["one"]  # => 1
```

Get all keys as an iterable with `keys()`. We need to wrap the call in `list()` to turn it into a list. We'll talk about those later. Note - for Python versions <3.7, dictionary key ordering is not guaranteed. Your results might not match the example below exactly. However, as of Python 3.7, dictionary items maintain the order at which they are inserted into the dictionary:

```py
list(filled_dict.keys())  # => ["three", "two", "one"] in Python <3.7
list(filled_dict.keys())  # => ["one", "two", "three"] in Python 3.7+
```

Get all values as an iterable with `values()`. Once again we need to wrap it in `list()` to get it out of the iterable. Note - Same as above regarding key ordering:

```py
list(filled_dict.values())  # => [3, 2, 1]  in Python <3.7
list(filled_dict.values())  # => [1, 2, 3] in Python 3.7+
```

Check for existence of keys in a dictionary with `in`:

```py
"one" in filled_dict  # => True
1 in filled_dict      # => False
```

Looking up a non-existing key is a `KeyError`:

```py
filled_dict["four"]  # KeyError
```

Use `get()` method to avoid the `KeyError`:

```py
filled_dict.get("one")      # => 1
filled_dict.get("four")     # => None
```

The get method supports a default argument when the value is missing:

```py
filled_dict.get("one", 4)   # => 1
filled_dict.get("four", 4)  # => 4
```

`setdefault()` inserts into a dictionary only if the given key isn't present:

```py
filled_dict.setdefault("five", 5)  # filled_dict["five"] is set to 5
filled_dict.setdefault("five", 6)  # filled_dict["five"] is still 5
```

Adding to a dictionary:

```py
filled_dict.update({"four":4})  # => {"one": 1, "two": 2, "three": 3, "four": 4}
filled_dict["four"] = 4         # another way to add to dict
```

Remove keys from a dictionary with `del`:

```py
del filled_dict["one"]  # Removes the key "one" from filled dict
```

From Python 3.5 you can also use the additional unpacking options:

```py
{'a': 1, **{'b': 2}}  # => {'a': 1, 'b': 2}
{'a': 1, **{'a': 2}}  # => {'a': 2}
```

## None

`None` is an object:

```py
None  # => None
```

Don't use the equality `==` symbol to compare objects to None
Use `is` instead. This checks for equality of object identity.

```py
"etc" is None  # => False
None is None   # => True
```

## Print

Python has a `print` function:

```py
print("I'm Python. Nice to meet you!")  # => I'm Python. Nice to meet you!
```

By default the `print` function also prints out a newline at the end. Use the optional argument end to change the end string:

```py
print("Hello, World", end="!")  # => Hello, World!
```

## User input

Get input data from console:

```py
input_string_var = input("Enter some data: ") # Returns the data as a string
```

## Ternary operator

If can be used as an expression. Equivalent of C's '?:' ternary operator

```py
result = "yay!" if 0 > 1 else "nay!"
print (result) # => "nay!"
```

## Sets

```py

```

## Control flow

```py

```

```py

```

```py

```

```py

```

```py

```

```py

```

```py

```

```py

```
