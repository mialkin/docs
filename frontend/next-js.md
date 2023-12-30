# Next.js

[↑ Next.js](https://nextjs.org) is an open-source web development framework created by the private company Vercel providing React-based web applications with server-side rendering and static website generation.

## Table of contents

- [Next.js](#nextjs)
  - [Table of contents](#table-of-contents)
  - [React](#react)
  - [`next.config.js` file](#nextconfigjs-file)

## React

[↑ React](https://react.dev), also known as **React.js** or **ReactJS**, is a free and open-source front-end JavaScript library for building user interfaces based on components. It is maintained by Meta (formerly Facebook) and a community of individual developers and companies.

React can be used to develop single-page, mobile, or server-rendered applications with frameworks like Next.js. Because React is only concerned with the user interface and rendering components to the DOM, React applications often rely on libraries for routing and other client-side functionality.

A key advantage of React is that it only rerenders those parts of the page that have changed, avoiding unnecessary rerendering of unchanged DOM elements.

## `next.config.js` file

The [↑ `next.config.js`](https://nextjs.org/docs/pages/api-reference/next-config-js) is a configuration file in the root of your project directory that configures Next.js.

`next.config.js` is a regular Node.js *module*, not a JSON file. It gets used by the Next.js server and build phases, and it's not included in the browser build.

A [↑ module](https://www.tutorialsteacher.com/nodejs/nodejs-modules) in Node.js is a simple or complex functionality organized in single or multiple JavaScript files which can be reused throughout the Node.js application.

Each module in Node.js has its own context, so it cannot interfere with other modules or pollute global scope. Also, each module can be placed in a separate `.js` file under a separate folder.
