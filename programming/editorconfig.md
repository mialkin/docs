# EditorConfig

[↑ EditorConfig](https://editorconfig.org) is a file format and collection of text editor plugins for maintaining consistent coding styles between different editors and IDEs.

`.editorconfig` is a configuration file that contains a list of rules which can be applied to any IDE's or code editors for proper formatting of code.

[↑ EditorConfig specification](https://editorconfig-specification.readthedocs.io/#supported-pairs)

## Table of contents

- [EditorConfig](#editorconfig)
  - [Table of contents](#table-of-contents)
  - [Why use EditorConfig](#why-use-editorconfig)
  - [Examples of use](#examples-of-use)
  - [File format](#file-format)
  - [Wildcard patterns](#wildcard-patterns)

## Why use EditorConfig

Consider a scenario where your team is working on a project but the individual members of the team use a different IDE or code editor, these differences might result in inconsistent formatting applied to the code as each IDE/editor has its own configurations. EditorConfig solves this problem by having a single config file that is readable by all IDE's and code editors with the help of some plugins/extensions.

## Examples of use

Some of the use cases:

- You can have a common indentation-style (Tabs/Spaces) & indentation-size
- EditorConfig will help in configuring character encoding and line-endings: LF/CRLF
- It can also enforce the editor to have a new line at end of each file

## File format

EditorConfig files have an INI-like file format.

Each line inside EditorConfig file must be one of the following:

- Blank line which contains only whitespace characters
- Comment which starts with a `;` or a `#`
- Section header which starts with a `[` and ends with a `]`
- Key-value pair which contains a key and a value, separated by an `=`

Example configuration file:

```ini
# EditorConfig is awesome: https://EditorConfig.org

# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
end_of_line = lf
insert_final_newline = true

# Matches multiple files with brace expansion notation
# Set default charset
[*.{js,py}]
charset = utf-8

# 4 space indentation
[*.py]
indent_style = space
indent_size = 4

# Tab indentation (no size specified)
[Makefile]
indent_style = tab

# Indentation override for all JS under lib directory
[lib/**.js]
indent_style = space
indent_size = 2

# Matches the exact files either package.json or .travis.yml
[{package.json,.travis.yml}]
indent_style = space
indent_size = 2
```

Whenever you open your editor, the EditorConfig plugin will look for a file named `.editorconfig` in the directory of opened files and in every parent directory. Search stops if it reaches the root file path which is specified by `root = true`.

Any rules specified under the `[*]` section header will be applied to all the files.

With the exception of the root key, all pairs must be located under a section to take effect. EditorConfig plugins ignore unrecognized keys and invalid/unsupported values for those keys.

## Wildcard patterns

Special characters recognized in section names for wildcard matching:

| Special character | Meaning                                                                                                           |
| ----------------- | ----------------------------------------------------------------------------------------------------------------- |
| \*                | Matches any string of characters, except path separators (/)                                                      |
| \*\*              | Matches any string of characters                                                                                  |
| ?                 | Matches any single character                                                                                      |
| [name]            | Matches any single character in name                                                                              |
| [!name]           | Matches any single character not in name                                                                          |
| {s1,s2,s3}        | Matches any of the strings given separated by commas                                                              |
| {num1..num2}      | Matches any integer numbers between `num1` and `num2`, where `num1` and `num2` can be either positive or negative |

Special characters can be escaped with a backslash so they won't be interpreted as wildcard patterns.
