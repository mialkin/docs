# Shell scripting

See where bash is by typing `which bash`. `#` is for "sharp" and `!` is for "bang" and `#!` is called "shebang". The form of a shebang interpreter directive is as follows:

```bash
#!interpreter [optional-arg]
```

in which interpreter is an absolute path to an executable program. The optional argument is a string representing a single argument. White space after `#!` is optional.

```bash
#! /bin/bash
```

## TOC

- [Shell scripting](#shell-scripting)
  - [TOC](#toc)
  - [Output text](#output-text)
  - [Variables](#variables)
    - [Default value](#default-value)
  - [User input](#user-input)
  - [Conditions](#conditions)
  - [Debug](#debug)

## Output text

Single line:

```bash
echo hello world
```

```bash
echo "hello world in double quotes"
```

Documents are often used to output multiline texts:

```bash
cat << EOF
Here is
a multiline

text
EOF
```

Using double quotes:

```bash
echo "But 
you can use

double quotes as well"
```

## Variables

Variables by convention should be uppercase. Only letters, numbers and undersore is allowed in the name of the variable:

```bash
NAME="Aleksei"
```

To use a variable you need to put a `$` in front of it:

```bash
echo $NAME
echo "My name is $NAME"
```

Curly braces work too

```bash
echo "My name is ${NAME}"
```

### Default value

If variable is not set or null, use default:

```bash
#NAME=John
NAME=
echo ${NAME:-Default value is Aleksei}
```

Assign default value:

```bash
echo ${NAME:=Default value is Aleksei}
echo $NAME
```

## User input

Reading user intput into a variable:

```bash
read -p "Enter your name: " NAME
echo Hello $NAME, nice to meet you!
```

## Conditions

Simple `if` statement:

```bash
NAME=Aleksei

if [ $NAME == Aleksei ]
then
    echo "Your name is Aleksei"
fi

```

`if-else`:

```bash
NAME=John

if [ $NAME == Aleksei ]
then
    echo "Your name is Aleksei"
else
    echo "Your name is not Aleksei"
fi
```

`else-if`:

```bash
if [ $NAME == Aleksei ]
then
    echo "Your name is Aleksei"
elif [ $NAME == John ]
then
    echo "Your name is John"
else
    echo "Your name is not Aleksei"
fi
```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```

## Debug

Either run entire script with `-x`:

```bash
bash -x ./script.sh
```

or wrap areas that you want to see what's happening with them with `set -x` and `set +x` to turn verbosity up/down:

```bash
#!/bin/bash

set -x
..code to debug...
set +x
```
