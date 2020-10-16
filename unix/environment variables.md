# Environment variables

Global **environment variables** are available to all processes and they are named in uppercase, with words joined by underscore (_), e.g., JAVA_HOME.

## Locals

Local variables aka **locals** are available to the current process only and they are in lowercase.

## Commands

Command | Description
-|-
env | List all environment variables. <br>`env` is an independent executable; it only sees the variables that the shell passes to it, or environment variables
set | List all environment varialbes + local viarables (locals). <br>`set` is a built-in shell command
a=1 | Set local variable `a`
unset a | Unset local variable `a`
export a=1 | Set environment variable `a`
echo $varname | Print value of `varname` environment variable

## Variables set by user

To open file with variables set by user on macOS run:

```sh
cd
vim .zprofile
```

After modifying .zprofile run `source .zprofile` to load new values immediately without having to reboot.