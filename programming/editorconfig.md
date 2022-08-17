# EditorConfig

[↑ EditorConfig](https://editorconfig.org) is a file format and collection of text editor plugins for maintaining consistent coding styles between different editors and IDEs.

`.editorconfig` is a configuration file that contains a list of rules which can be applied to any IDE's or code editors for proper formatting of code.

## Why use EditorConfig

Consider a scenario where your team is working on a project but the individual members of the team use a different IDE or code editor, these differences might result in inconsistent formatting applied to the code as each IDE/editor has its own configurations. EditorConfig solves this problem by having a single config file that is readable by all IDE’s and code editors with the help of some plugins/extensions.

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

[*]
charset = utf-8
end_of_line = lf
indent_size = 4
indent_style = space
insert_final_newline = true
max_line_length = 80
trim_trailing_whitespace = true

[*.md]
max_line_length = 0
trim_trailing_whitespace = false 
```

Whenever you open your editor, the EditorConfig plugin will look for a file named `.editorconfig` in the directory of opened files and in every parent directory. Search stops if it reaches the root file path which is specified by `root = true`.

Any rules specified under the `[*]` section header will be applied to all the files. You can have specific rules for specific files for example [*.md] section header has the rules which should be applied on only files having `.md` as an extension.
