# Modules in go

Modules in Go is an `unique` reference for your project. It's managed by the
go.mod file and it has all the dependencies the project relies on. It is
basically a dependency manager, like NPM is for JavaScript. It describes the
list of packages that your application needs to run.

If you want to create a dependency for other to use the module name `has to be
your github/gitlab path`.

A file go.sum is added to when installing some dependencies to the project since
it's a file that will contain a hash string with the encrypted bits of the
source code identification and by the `checksum` process it will trigger any
changes in the checked files. It's a lock file for versions. It will contain
also a dependency tree of each downloaded package.

## Structure

```terminal
go mod init prefix/project-name
```

## Imports

Imports in go are a relative path to the main module until the last package that
contains your file import. The import can be a variable, a function, a struct
but it has to have a `Capital Letter` at the start.

## Libraries

To import a lib into your program, or a package as you wish, we just need to
type the following command in the CLI

```terminal
go get PATH
`
```
