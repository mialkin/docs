# Environment variables

File with variables set by user.

```sh
cd
vim .zprofile
```

After modifying .zprofile run `source .zprofile` to load new values immediately without having to reboot.

## Environment variables vs locals

Global environment variables (available to all processes) are named in uppercase, with words joined with underscore (_), e.g., JAVA_HOME. Local variables (available to the current process only) are in lowercase.

## Commands

Command | Description
-|-
env | List all environment variables. <br>`env` is an independent executable; it only sees the variables that the shell passes to it, or environment variables
set | List all environment varialbes + local viarables (locals). <br>`set` is a built-in shell command
a=1 | Set local variable `a`
unset a | Unset local variable `a`
export a=1 | Set environment variable `a`
echo $varname | Print value of `varname` environment variable
