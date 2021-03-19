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
