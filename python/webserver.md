# Web server

Start new web server.

```console
python3 -m http.server
```

## Misc

Make an alias for python3.

```sh
echo "alias python=python3" >> ~/.zshrc
```

Print paths where Python is looking for modules.

```sh
import sys

print(sys.path)
```