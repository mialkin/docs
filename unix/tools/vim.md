# Vim

Display line numbers by default:

```bash
echo "set number" >> ~/.vimrc
source ~/.vimrc
```

## Commands

| Command       | Explanation                  |
| ------------- | ---------------------------- |
| :w            | Write changes                |
| :q            | Quit                         |
| :q!           | Quit without saving          |
| :set          | List everything you have set |
| :set number   | Display line numbers         |
| :set nonumber | Hide line numbers            |

## Cursor movements

| Keys    | Explanation                |
| ------- | -------------------------- |
| h j k l | Move cursor                |
| b       | Go to begining of word     |
| w       | Go to end of word          |
| ^       | Go to begining of line     |
| $       | Go to end of line          |
| gg      | Go to begining of document |
| G       | Go to end of document      |
| 0       | Move to beginning of line  |

## Editing

| Keys | Explanation                                              |
| ---- | -------------------------------------------------------- |
| u    | Undo                                                     |
| dd   | Delete line                                              |
| D    | Delete to end of line                                    |
| i    | Insert before the cursor                                 |
| I    | Insert at the beginning of the line                      |
| a    | Insert after the cursor                                  |
| A    | Insert at the end of the line                            |
| o    | Insert new line below                                    |
| O    | Insert new line above                                    |
| cw   | Delete everything to the end of the word                 |
| ca\[ | Delete everything between brackets                       |
| C    | Delete everything from cursor to the end of line         |
| yy   | Copy to clipboard the current line                       |
| p    | Paste the copied or deleted text after the current line  |
| P    | Paste the copied or deleted text before the current line |
