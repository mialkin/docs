# EditorConfig

**EditorConfig** is a file format and collection of text editor plugins for maintaining consistent coding styles between different editors and IDEs.

**`.editorconfig`** is a configuration file that contains a list of rules which can be applied to any IDE's or code editors for proper formatting of code.

## Why use EditorConfig

Consider a scenario where your team is working on a project but the individual members of the team use a different IDE or code editor, these differences might result in inconsistent formatting applied to the code as each IDE/editor has its own configurations. EditorConfig solves this problem by having a single config file that is readable by all IDE’s and code editors with the help of some plugins/extensions.

## Examples of use

Some of the use cases:

- You can have a common indentation-style (Tabs/Spaces) & indentation-size
- EditorConfig will help in configuring character encoding and line-endings: LF/CRLF
- It can also enforce the editor to have a new line at end of each file

## File format

EditorConfig files are in an INI-like file format. In an EditorConfig file, all beginning whitespace on each line is considered irrelevant. Each line must be one of the following:

- Blank: contains only whitespace characters
- Comment: starts with a `;` or a `#`
- Section header: starts with a `[` and ends with a `]`
- Key-value pair: contains a key and a value, separated by an `=`
