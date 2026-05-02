# GoLand

## `goland` command

Create file:

```bash
cd /usr/local/bin/ && \
sudo vim goland
```

With content:

```text
#!/bin/sh

open -na "GoLand.app" --args "$@"
```

Change access mode:

```bash
sudo chmod 775 goland
```
