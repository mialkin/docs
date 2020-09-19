# tmux

## Commands

Command | Description
-|-
tmux | Create session without name 0, 1, 2, ...
tmux new -s test | Create new session named `test`
tmux attach | Attach to a single existing session or to the last created session
tmux attach -t test | Attach to a session named `test`

## Hotkeys

Hotkey | Description
-|-
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>C</kbd> | Create new window
<kbd>Ctrl</kbd>+<kbd>B</kbd> + <kbd>1</kbd>| Go to window number 1
<kbd>Ctrl</kbd>+<kbd>B</kbd> + <kbd>,</kbd>| Rename current window
<kbd>Ctrl</kbd>+<kbd>B</kbd> + <kbd>X</kbd>| Close current pane/window
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>%</kbd> | Split window vertically
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>"</kbd> | Split window horizontally
<kbd>Ctrl</kbd>+<kbd>B</kbd>, <kbd>Arrow keys</kbd> | Jump between panels inside window
<kbd>Ctrl</kbd>+<kbd>B</kbd>+<kbd>D</kbd> | Detach from session
<kbd>Ctrl</kbd>+<kbd>B</kbd>+<kbd>S</kbd> | Select session
