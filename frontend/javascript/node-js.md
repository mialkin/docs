# Node.js, Next.js

## Table of contents

- [Node.js, Next.js](#nodejs-nextjs)
  - [Table of contents](#table-of-contents)
  - [Node.js](#nodejs)
    - [npm](#npm)
      - [`package.json`](#packagejson)
        - [`name`](#name)
        - [`version`](#version)
        - [`private`](#private)
        - [`scripts`](#scripts)
        - [`dependencies`](#dependencies)
        - [`devDependencies`](#devdependencies)
      - [Packages](#packages)
        - [`eslint`](#eslint)
        - [`eslint-config-next`](#eslint-config-next)
        - [Babel](#babel)
      - [Scope](#scope)
  - [Next.js](#nextjs)
    - [React](#react)
    - [`next.config.js` file](#nextconfigjs-file)

## Node.js

[↑ Node.js](https://nodejs.org) is a cross-platform, open-source JavaScript runtime environment.

Node.js runs on the [↑ V8](https://en.wikipedia.org/wiki/V8_(JavaScript_engine)) [↑ JavaScript engine](https://en.wikipedia.org/wiki/JavaScript_engine), and executes JavaScript code outside a web browser.

### npm

**npm**, short for Node package manager, is the dependency/package manager you get out of the box when you install Node.js.

npm consists of three distinct components:

- the website
- the CLI
- the registry

#### `package.json`

The `package.json` file contains descriptive and functional metadata about a project, such as a `name`, `version`, and `dependencies`.

The file provides the npm package manager with various information to help identify the project and handle dependencies.

The `npm init` command walks you through creating a `package.json` file.

##### `name`

The [↑ `name`](https://docs.npmjs.com/cli/v10/configuring-npm/package-json#name) property is a descriptive field that identifies a project. The combination of a project `name` and `version` forms a unique identifier for a package.

The maximum length of a name is 214 characters. Only lowercase letters are allowed in a name. For published packages, the name must be unique.

##### `version`

The [↑ `version`](https://docs.npmjs.com/cli/v10/configuring-npm/package-json#version) property is a descriptive field that helps identify the package version. A published project requires having a version field in the `package.json` file. If working on a personal project, this property is optional.

##### `private`

The [↑ `private`](https://docs.npmjs.com/cli/v10/configuring-npm/package-json#private) property set to `true` disallows npm to publish package to registry.

This is a way to prevent accidental publication of private repositories.

##### `scripts`

The [↑ `scripts`](https://docs.npmjs.com/cli/v10/using-npm/scripts) property is a dictionary containing script commands that are run at various times in the lifecycle of your package. The key is the lifecycle event, and the value is the command to run at that point.

##### `dependencies`

The [↑ `dependencies`](https://docs.npmjs.com/cli/v10/configuring-npm/package-json#dependencies ) property is a simple object that maps a package name to a version range.

##### `devDependencies`

The [↑ `devDependencies`](https://docs.npmjs.com/cli/v10/configuring-npm/package-json#devdependencies) property specifies those packages in the `package.json` file that you need only for project development purposes.

#### Packages

##### `eslint`

[↑ ESLint](https://eslint.org) is a tool for identifying and reporting on patterns found in ECMAScript/JavaScript code

##### `eslint-config-next`

The [↑ `eslint-config-next`](https://nextjs.org/docs/app/building-your-application/configuring/eslint) package includes everything you need to have an optimal out-of-the-box linting experience in Next.js.

##### Babel

[↑ Babel](https://babeljs.io) is a JavaScript compiler.

Babel is mainly used to convert ECMAScript 2015+ code into a backwards compatible version of JavaScript in current and older browsers or environments.

#### Scope

A [↑ scope](https://docs.npmjs.com/cli/v9/using-npm/scope) is a way of grouping related packages together, and also affect a few things about the way npm treats the package.

## Next.js

[↑ Next.js](https://nextjs.org) is an open-source web development framework created by the private company Vercel providing React-based web applications with server-side rendering and static website generation.

The easiest way to get started with Next.js is by using [↑ `create-next-app`](https://nextjs.org/docs/app/api-reference/create-next-app) CLI tool:

```bash
npx create-next-app@latest
```

### React

[↑ React](https://react.dev), also known as **React.js** or **ReactJS**, is a free and open-source front-end JavaScript library for building user interfaces based on components. It is maintained by Meta (formerly Facebook) and a community of individual developers and companies.

React can be used to develop single-page, mobile, or server-rendered applications with frameworks like Next.js. Because React is only concerned with the user interface and rendering components to the DOM, React applications often rely on libraries for routing and other client-side functionality.

A key advantage of React is that it only rerenders those parts of the page that have changed, avoiding unnecessary rerendering of unchanged DOM elements.

### `next.config.js` file

The [↑ `next.config.js`](https://nextjs.org/docs/pages/api-reference/next-config-js) is a configuration file in the root of your project directory that configures Next.js.

`next.config.js` is a regular Node.js *module*. It gets used by the Next.js server and build phases, and it's not included in the browser build.

A [↑ module](https://www.tutorialsteacher.com/nodejs/nodejs-modules) in Node.js is a simple or complex functionality organized in single or multiple JavaScript files which can be reused throughout the Node.js application.

Each module in Node.js has its own context, so it cannot interfere with other modules or pollute global scope. Also, each module can be placed in a separate `.js` file under a separate folder.
