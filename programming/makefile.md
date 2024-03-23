# Makefile

**Make** is a build automation tool that automatically builds executable programs and libraries from source code by reading files called _Makefiles_ which specify how to derive the target program.

GNU Make searches files in order for a file named one of `GNUmakefile`, `makefile`, or `Makefile` and then runs the specified (or default) target(s) from (only) that file.

[↑ What is a Makefile and how does it work?](https://opensource.com/article/18/8/what-how-makefile)

## Table of contents

- [Makefile](#makefile)
  - [Table of contents](#table-of-contents)
  - [Syntax](#syntax)
  - [Makefiles vs shell scripts](#makefiles-vs-shell-scripts)
    - [Simplicity](#simplicity)
    - [Minimal rebuilds](#minimal-rebuilds)

## Syntax

Example of a Makefile:

```makefile
say_hello:
        echo "Hello World"
```

A **rule** is a set of a _target_, _prerequisites_, and _recipes_.

Syntax of a typical rule:

```makefile
target: prerequisites
<TAB> recipe
```

A **target** might be a binary file that depends on prerequisites (source files).

A **phony target** is a target that's not a file.

A **prerequisite** can also be a target that depends on other dependencies.

A **recipe** is a command inside a rule.

## Makefiles vs shell scripts

### Simplicity

Makefiles give simplicity to users.

You can have Makefiles in all of your projects so that developers can: `make build`, `make test`, `make run`, `make push`, `make deploy`, and not worry about what the command is to build this project, run the test suite, run the project itself, create a Docker container and push it to an image registry, or deploy it to Kubernetes. All of those details are hidden in the Makefile and every project has the same flow whether it's Python, Java, Node, etc.

[↑ Makefiles vs. Bash Scripts](https://www.reddit.com/r/devops/comments/z1eegq/makefiles_vs_bash_scripts/).

### Minimal rebuilds

The general idea is that `make` supports reasonably minimal rebuilds — i.e., you tell it what parts of your program depend on what other parts. When you update some part of the program, it _only_ rebuilds the parts that depend on that. While you _could_ do this with a shell script, it would be a lot more work: explicitly checking the last-modified dates on all the files, etc. The only obvious alternative with a shell script is to rebuild everything every time. For tiny projects this is a perfectly reasonable approach, but for a big project a complete rebuild could easily take an hour or more — using `make`, you might easily accomplish the same thing in a minute or two...

[↑ Why use make over a shell script?](https://stackoverflow.com/questions/3798562/why-use-make-over-a-shell-script).
