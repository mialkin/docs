# Docusaurus, docfx

## Docusaurus

[↑ Docusaurus](https://docusaurus.io) is an optimized site generator in React.

Docusaurus builds a single-page application with fast client-side navigation, leveraging the full power of React to make a site interactive.

Docusaurus provides out-of-the-box documentation features but can be used to create any kind of site: personal website, product, blog, marketing landing pages, etc.

Installation:

```bash
npx create-docusaurus@latest my-website classic
```

## docfx

[↑ docfx](https://dotnet.github.io/docfx/) is an open-source static site generator maintained by the .NET Foundation, primarily designed for building and publishing documentation.

It excels at generating API documentation from .NET source code (including C#, VB.NET), XML code comments, and REST API Swagger files. Beyond API documentation, DocFX can also convert Markdown files into a comprehensive documentation website.

Install the latest docfx:

```bash
dotnet tool update --global docfx
```

Create a new docset, run:

```bash
docfx init
```

This command walks you through creating a new docfx project under the current working directory.

Build the docset:

```bash
docfx docfx.json --serve
```

Now you can preview the website on <http://localhost:8080>.

To preview your local changes, save changes then run this command in a new terminal to rebuild the website:

```bash
docfx docfx.json
```
