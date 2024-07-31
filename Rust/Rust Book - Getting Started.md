---
created at: 2024-05-21
tags:
  - setup
---
***

### Install and Basics

To install rust:
Linux or macOS:
`$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh`
Windows: must go to rust lang and install from there.

`rustc` is the command rust language uses, like python or java.

`rustc --version` to check which version of rust we have.

`rustup update` to update version of rust.]

`rustup self uninstall` to uninstall rust.

`rustup doc` to access the rust documentation offline.

`rustfmt` formats all rust code in a project to a specified style.

You can use `rustc` to compile rust source code and then execute the compiles file but it is better to use `cargo` which function like node's `npm`.

`cargo` == `npm`
`Cargo.toml` == `package.json`
`Cargo.lock` == `package-lock.json`

> [!info]
>All executable rust files must contain a `main` function.
>Functions with an `!` before the parameters are [[macros]].
>```rust
> println!("Hello World!") // macro call
> println("HelloWorld!") // function call
>```
>Rust also uses 4 spaces, not tabs, for spacing


### Cargo

[[Cargo commands|commands]]

As before Cargo is like node's npm package manager. When you install rust Cargo is included.




