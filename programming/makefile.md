# Makefile

**Make** is a build automation tool that automatically builds executable programs and libraries from source code by reading files called **Makefiles** which specify how to derive the target program.

GNU Make searches files in order for a file named one of `GNUmakefile`, `makefile`, or `Makefile` and then runs the specified (or default) target(s) from (only) that file.

[↑ What is a Makefile and how does it work?](https://opensource.com/article/18/8/what-how-makefile)

## Syntax

Example of a Makefile:

```makefile
say_hello:
        echo "Hello World"
```

A **rule** is a set of a *target*, *prerequisites*, and *recipes*.

Syntax of a typical rule:

```makefile
target: prerequisites
<TAB> recipe
```

A **target** might be a binary file that depends on prerequisites (source files).

A **phony target** is a target that's not a file.

A **prerequisite** can also be a target that depends on other dependencies.

A **recipe** is a command inside a rule.
