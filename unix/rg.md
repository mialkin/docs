# rg

The **rg** tool allows to search current directory recursively for lines matching a pattern.

Search "ssh" keyword.

```sh
ll | rg ssh
find . | rg .sln
find "$PWD" | rg .sln
ls -R | rg .sln
```
