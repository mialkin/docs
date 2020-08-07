# Definitions

## Grep

The **grep** is a command-line utility for searching plain-text data sets for lines that match a regular expression. Its name comes from the [ed](https://en.wikipedia.org/wiki/Ed_(text_editor)) command *g/re/p* (_**g**lobally search for a **r**egular **e**xpression and **p**rint matching lines_), which has the same effect. grep was originally developed for the Unix operating system, but later available for all Unix-like systems and some others

## Regular expression

A **regular expression** (shortened as **regex** or **regexp**) is a sequence of characters that define a search pattern.

## Unix pipeline

In Unix-like computer operating systems, a pipeline is a mechanism for inter-process communication using message passing. A pipeline is a set of processes chained together by their standard streams, so that the output text of each process (stdout) is passed directly as input (stdin) to the next one. The second process is started as the first process is still executing, and they are executed concurrently. The concept of pipelines was championed by Douglas McIlroy at Unix's ancestral home of Bell Labs, during the development of Unix, shaping its toolbox philosophy.

The standard shell syntax for anonymous pipes is to list multiple commands, separated by vertical bars ("pipes" in common Unix verbiage):

```console
process1 | process2 | process3
```

For example, to list files in the current directory (ls), retain only the lines of ls output containing the string "key" (grep), and view the result in a scrolling page (less), a user types the following into the command line of a terminal:

```console
ls -l | grep key | less
```
