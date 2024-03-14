# Shell and shell scripting

## Table of contents

- [Shell and shell scripting](#shell-and-shell-scripting)
  - [Table of contents](#table-of-contents)
  - [Shell](#shell)
    - [Bash](#bash)
    - [Bash commands](#bash-commands)
  - [Shell scripting](#shell-scripting)
    - [Shebang](#shebang)
    - [Folder for user scripts](#folder-for-user-scripts)
    - [Executing script](#executing-script)
    - [Output text](#output-text)
    - [Variables](#variables)
      - [Default value](#default-value)
      - [Variable interpolation](#variable-interpolation)
    - [User input](#user-input)
    - [Conditions](#conditions)
    - [Integer comparison](#integer-comparison)
    - [File conditions](#file-conditions)
    - [`case` statement](#case-statement)
    - [`for` loop](#for-loop)
    - [`while` loop](#while-loop)
    - [Functions](#functions)
    - [Pass arguments to script](#pass-arguments-to-script)
    - [Debug](#debug)
    - [Open terminal in macOS](#open-terminal-in-macos)
    - [Links](#links)

## Shell

In computing, a **shell** is a user interface for access to an operating system's services. In general, operating system shells use either a command-line interface, CLI or graphical user interface, GUI, depending on a computer's role and particular operation. It is named a shell because it is the outermost layer around the operating system.

Command-line shells require the user to be familiar with commands and their calling syntax, and to understand concepts about the shell-specific scripting language, for example, bash.

### Bash

**Bash**, GNU Bourne-Again Shell, is an sh-compatible command language interpreter that executes commands read from the standard input or from a file. Bash also incorporates useful features from the Korn and C shells, ksh and csh.

### Bash commands

| Command             | Description                                                                                                                                       |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| bash                | Switch to a bash or different shell type its name at the terminal                                                                                 |
| cat /etc/shells     | Available shells                                                                                                                                  |
| cat /etc/\*-release | Display OS name and version                                                                                                                       |
| echo $#             | `$#` is a special variable in bash, that expands to the number<br/> of arguments (positional parameters) i.e. `$1`, `$2` ... passed to the script |
| echo $?             | Print exit status of previously executed command                                                                                                  |
| echo $0             | Print the name of currently used shell                                                                                                            |
| echo $SHELL         | Display what shell the terminal opened with                                                                                                       |
| export VAR=VAL      | Mark a shell variable for export to child processes                                                                                               |
| first \|\| second   | If the exit status of the first command is not 0, then execute<br/> the second command (exit 2)                                                   |
| hostnamectl         | OS info                                                                                                                                           |
| lsb_release -a      | Print certain LSB (Linux Standard Base) and<br/> distribution-specific information                                                                |
| uname -a            | Kernel version                                                                                                                                    |

## Shell scripting

A **shell script** is a computer program designed to be run by a Unix shell, a command-line interpreter.

The various dialects of shell scripts are considered to be scripting languages.

Typical operations performed by shell scripts include file manipulation, program execution, and printing text.

### Shebang

`#` is for "sharp" and `!` is for "bang" and `#!` is called "shebang". 

The form of a shebang interpreter directive is as follows:

```bash
#!interpreter [optional-arg]
```

in which interpreter is an absolute path to an executable program. The optional argument is a string representing a single argument.

White space after `#!` is optional:

```bash
#!/bin/bash
```

See where bash is by typing `which bash`.

### Folder for user scripts

Conventional place, and this directory should be empty on fresh installs:

```bash
cd /usr/local/bin
touch script.sh
```

### Executing script

```bash
# Add execution permission to user:
chmod 100 script.sh

# Execute script:
./script.sh

# You can also use bash script.sh or sh script.sh
```

### Output text

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

### Variables

Variables by convention should be uppercase. Only letters, numbers and underscore is allowed in the name of the variable:

```bash
NAME="Aleksei"
```

To use a variable you need to put a `$` in front of it:

```bash
echo $NAME
echo "My name is $NAME"
```

Curly braces work too:

```bash
echo "My name is ${NAME}"
```

#### Default value

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

#### Variable interpolation

```bash
FILE_NAME="logs"
echo "$HOME/${FILE_NAME}_2021.txt"  # /Users/aleksei/logs_2021.txt
echo "$HOME/$FILE_NAME_2021.txt"    # /Users/aleksei/.txt
```

### User input

Reading user input into a variable:

```bash
read -p "Enter your name: " NAME
echo Hello $NAME, nice to meet you!
```

### Conditions

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

### Integer comparison

You can use `-eq`, `-ne`, `-gt`, `-ge`, `-lt`, `-le` for comparison:

```bash
NUM1=3
NUM2=5

if [ "$NUM1" -gt "$NUM2" ]
then
    echo "NUM1 is greater than NUM2"
else
    echo "NUM1 is less than NUM2"
fi
```

You can also use `==`, `!=`, `>`, `>=`, `<`, `<=`:

```bash
NUM1=3
NUM2=5

if (("$NUM1" > "$NUM2"))
then
    echo "NUM1 is greater than NUM2"
else
    echo "NUM1 is less than NUM2"
fi
```

### File conditions

```bash
FILE="test.txt"
if [ -e "$FILE" ]
then
  echo "$FILE exists"
else
  echo "$FILE does NOT exist"
fi
```

| Flag | Description                                                                                       |
| ---- | ------------------------------------------------------------------------------------------------- |
| -d   | True if the file is a directory                                                                   |
| -e   | True if the file exists. Note that this is not particularly portable, thus `-f` is generally used |
| -f   | True if the provided string is a file                                                             |
| -g   | True if the group id is set on a file                                                             |
| -r   | True if the file is readable                                                                      |
| -s   | True if the file has a non-zero size                                                              |
| -u   | True if the user id is set on a file                                                              |
| -w   | True if the file is writable                                                                      |
| -x   | True if the file is an executable                                                                 |

### `case` statement

```bash
read -p "Are you 21 or over? Y/N " ANSWER
case "$ANSWER" in
[yY] | [yY][eE][sS])
    echo "You can have a beer :)"
    ;;
[nN] | [nN][oO])
    echo "Sorry, no drinking"
    ;;
*)
    echo "Please enter y/yes or n/no"
    ;;
esac
```

### `for` loop

Simple `for` loop:

```bash
NAMES="Brad Kevin Alice Mark"
for NAME in $NAMES; do
    echo "Hello $NAME"
done
```

`for` loop to rename files:

```bash
touch 1.txt 2.txt 3.txt

FILES=$(ls *.txt)
NEW="new"
for FILE in $FILES; do
    echo "Renaming $FILE to $NEW-$FILE"
    mv $FILE $NEW-$FILE
done

rm new-1.txt new-2.txt new-3.txt
```

### `while` loop

Read through a file line by line:

```bash
LINE=1
while read -r CURRENT_LINE; do
    echo "$LINE: $CURRENT_LINE"
    ((LINE++))
done <"./new-1.txt"
```

Run command infinitely with 2 seconds delay after each iteration:

```bash
while true; do foo; sleep 2; done
```

### Functions

Simple function:

```bash
function sayHello() {
    echo "Hello World"
}

sayHello
```

Function with params:

```bash
function greet() {
  echo "Hello, I am $1 and I am $2"
}

greet "John" "36"
```

### Pass arguments to script

```bash
#!/bin/bash

if [ $# -ne 1 ]; then
    echo $0: usage: myscript name
    exit 1
fi

echo $1
```

### Debug

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

### Open terminal in macOS

Open terminal and run commands:

```bash
#!/bin/bash

osascript -e 'tell app "Terminal" to do script "cd ~/app-ui/ &&
clear &&
npm run dev
"'

osascript -e 'tell app "Terminal" to do script "cd ~/app-api/src/App.Api/ &&
clear &&
dotwatch
"'
```

### Links

- [↑ Learn bash in Y minutes](https://learnxinyminutes.com/docs/bash/)
- [↑ Bash scripting cheatsheet](https://devhints.io/bash)
