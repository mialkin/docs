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

## macOS variables set by user

Environment variables set like this are only stored temporally. When you exit the running instance of bash by exiting the terminal, they get discarded. To save them permanentally, open the file:

```bash
cd
vim .zprofile
source .zprofile
```

The `source .zprofile` command loads new values immediately without having to reboot system.

## Ubuntu global variables

```bash
cd
sudo vim /etc/environment
```

Example of environment file:

```text
export ADMIN_USERNAME=admin
export ADMIN_PASSWORD=password123
```

Load new values without having ot reboot:

```bash
source /etc/environment
env
```
