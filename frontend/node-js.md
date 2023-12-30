# Node.js

[↑ Node.js](https://nodejs.org) is a cross-platform, open-source JavaScript runtime environment.

Node.js runs on the [↑ V8](https://en.wikipedia.org/wiki/V8_(JavaScript_engine)) [↑ JavaScript engine](https://en.wikipedia.org/wiki/JavaScript_engine), and executes JavaScript code outside a web browser.

## Table of contents

- [Node.js](#nodejs)
  - [Table of contents](#table-of-contents)
  - [npm](#npm)
    - [`package.json`](#packagejson)
      - [`name`](#name)
      - [`version`](#version)
      - [`private`](#private)

## npm

**npm**, short for Node package manager, is the dependency/package manager you get out of the box when you install Node.js.

npm consists of three distinct components:

- the website
- the CLI
- the registry

### `package.json`

The `package.json` file contains descriptive and functional metadata about a project, such as a `name`, `version`, and `dependencies`.

The file provides the npm package manager with various information to help identify the project and handle dependencies.

#### `name`

The [↑ `name`](https://docs.npmjs.com/cli/v10/configuring-npm/package-json#name) property is a descriptive field that identifies a project. The combination of a project `name` and `version` forms a unique identifier for a package.

The maximum length of a name is 214 characters. Only lowercase letters are allowed in a name. For published packages, the name must be unique.

#### `version`

The [↑ `version`](https://docs.npmjs.com/cli/v10/configuring-npm/package-json#version) property is a descriptive field that helps identify the package version. A published project requires having a version field in the `package.json` file. If working on a personal project, this property is optional.

#### `private`

If you set [↑ `private`](https://docs.npmjs.com/cli/v10/configuring-npm/package-json#private) property to `true` npm will refuse to publish package to registry.

This is a way to prevent accidental publication of private repositories.
