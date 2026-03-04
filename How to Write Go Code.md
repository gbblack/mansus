---
tags:
DECIMAL: 41.25 Go
source: https://go.dev/doc/code
links:
  - "[[Go]]"
---
# How to Write Go Code

> [!abstract] Summary

## Code Organisation

- **Packages.** A package is a collection of source files in the same directory. All definitions in one source file is visible to all other files in the same package.
- **Module.** A repository may contain one or more modules. A module is a collection of packages released together.
  - A repository typically contains a single module, at the root of the project.
  - `go.mod` file declares the module path -> the import path prefix for all packages within the module.
  - A module contains all packages that are in the same directory containing `go.mod`. This includes all subdirectories until another `go.mod` file is reached (if any).
- **Import Path.** An import path is the string used to import a package. It is defined as: `module path + package subdirectory`. E.g. `github.com/google/go-cmp/cmp`
  - You do not need to publish code to build it, but it is best practice to define and organise code as if you were going to publish it.
  - A module path is simply the import prefix for its package. It also defines where the [[go command]] should look to download it. This is why the remote forge is included in the path -> `golang.org/x/tools` will look to download the repository at `https://golang.org/x/tools`.
  - Packages in the standard library do not have a module path prefix. You can import them like this -> `log` or `net/http`.