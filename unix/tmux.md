# tmux

**tmux** is a terminal multiplexer for Unix-like operating systems. It allows multiple terminal sessions to be accessed simultaneously in a single window. It is useful for running more than one command-line program at the same time. It can also be used to detach processes from their controlling terminals, allowing remote sessions to remain active without being visible.

## Installing

To install `tmux` run:

```bash
sudo apt install -y tmux
```

## Commands

Command                        | Description
-------------------------------|---------------------------------------------------
tmux                           | Create session without name 0, 1, 2, ...
tmux ls                        | List sessions
tmux new -s test               | Create new session named `test`
tmux new-session -t testing -d | Start new session if `no server running on /private/tmp/tmux-501/default` message
tmux attach                    | Attach to a single existing session or to the last created session
tmux attach -t test            | Attach to a session named `test`
tmux -V                        | Show version of the program

## Hotkeys

Hotkey                                              | Description
----------------------------------------------------|----------------------------------
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>D</kbd>           | Detach from session
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>C</kbd>          | Create new window
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>1</kbd>         | Go to window number 1
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>,</kbd>         | Rename current window
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>X</kbd>         | Close current pane/window
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>%</kbd>          | Split window vertically
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>"</kbd>          | Split window horizontally
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>Arrow keys</kbd> | Jump between panels inside window
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>S</kbd>           | Select session


## Configuration file

Open tmux config file.

```sh
cd
vim .tmux.config
```

Paste following line to enable mouse.

```text
set -g mouse on
```
