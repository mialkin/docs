# Go

[↑ Go](<https://en.wikipedia.org/wiki/Go_(programming_language)>) is an open-source high-level general purpose programming language that is statically typed and compiled.

Go was publicly announced in November 2009, and version 1.0 was released in March 2012.

The lack of support for generic programming in initial versions of Go drew considerable criticism. Generics were finally added to Go in version 1.18 in March 2022.

Go uses a `go1.[major].[patch]` versioning format, such as `go1.24.0` and each major Go release is supported until there are two newer major releases. Unlike most software, Go calls the second number in a version the major, i.e., in `go1.24.0` the `24` is the major version. This is because Go plans to never reach 2.0, prioritizing backwards compatibility over potential breaking changes.

The Go authors [↑ put substantial effort](<https://en.wikipedia.org/wiki/Go_(programming_language)#Style>) into influencing the style of Go programs.

## Installation

[↑ Download page](https://go.dev/doc/install).

## GoLand

### `goland` command

Create file:

```bash
cd /usr/local/bin/ && \
sudo vim goland
```

With content:

```text
#!/bin/sh

open -na "GoLand.app" --args "$@"
```

Change access mode:

```bash
sudo chmod 775 goland
```
