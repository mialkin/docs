# Shell

In computing, a **shell** is a user interface for access to an operating system's services. In general, operating system shells use either a command-line interface (CLI) or graphical user interface (GUI), depending on a computer's role and particular operation. It is named a shell because it is the outermost layer around the operating system.

Command-line shells require the user to be familiar with commands and their calling syntax, and to understand concepts about the shell-specific scripting language (for example, bash).

## Bash

**Bash** (GNU Bourne-Again SHell) is an **sh**-compatible command language interpreter that executes commands read from the standard input or from a file. Bash also incorporates useful features from the Korn and C shells (ksh and csh).

## Bash commands

| Command             | Description                                                                                |
| ------------------- | ------------------------------------------------------------------------------------------ |
| bash                | Switch to a bash or different shell type its name at the terminal                          |
| cat /etc/shells     | Available shells                                                                           |
| cat /etc/\*-release | Display OS name and version                                                                |
| echo $?             | Print exit status of previously executed command                                           |
| echo $0             | Print the name of currently used shell                                                     |
| echo $SHELL         | Display what shell the terminal opened with                                                |
| export VAR=VAL      | Mark a shell variable for export to child processes                                        |
| first \|\| second   | If the exit status of the first command is not 0, then execute the second command (exit 2) |
| hostnamectl         | OS info                                                                                    |
| lsb_release -a      | Print certain LSB (Linux Standard Base) and distribution-specific information              |
| uname -a            | Kernel version                                                                             |
