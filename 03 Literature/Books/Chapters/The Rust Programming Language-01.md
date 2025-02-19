---
tags:
  - type/chapter
  - status/day
created at: 2024-05-21
---
[[The Rust Programming Language]]
# Getting Started

> [!abstract] Summary
> Introduction to Rust and Cargo with instructions on installation and basic project setup. 
## Note
---
> [!tip]- Table of Contents
> 1. [[#Installation]]
> 	1. [[#Installing `rustup` on Linux or macOs]]
> 	2. [[#Installing `rustup` on Windows]]
> 	3. [[#Troubleshooting]]
> 	4. [[#Updating and Uninstalling]]
> 	5. [[#Local Documentation]]
> 2. [[#Hello, World!]]
> 	1. [[#Writing and Running a Rust Program]]
> 	2. [[#Anatomy of a Rust Program]]
> 	3. [[#Compiling and Running Are Separate Steps]]
> 3. [[#Hello, Cargo!]]
> 	1. [[#Creating a Project with Cargo]]
> 	2. [[#Building and Running a Cargo Project]]
> 	3. [[#Building for Release]]
> 	4. [[#Cargo as Convention]]
### Installation
- Rust is installed through `rustup` a command line tool for managing Rust versions and tooling.
#### Installing `rustup` on Linux or macOs
- `$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh`.
  This command installs `rustup` which then installs the latest version of Rust.
  A *linker* is also required, usually it is already installed. If not macOS uses: `$ xcode-select --install` and Linux will use different ones depending on the distro.
#### Installing `rustup` on Windows
- Follow the instructions from this [link]([https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install).
#### Troubleshooting
- To check install: `rustc --version`.
- To check that Rust is in PATH: `echo %PATH%` (Win) or `echo $PATH` (Unix).
#### Updating and Uninstalling
- To update Rust version: `rustup update`.
- To uninstall Rust and `rustup`: `rustup self uninstall`.
#### Local Documentation
- To view Rust documentation: `rustup doc`.
### Hello, World!
#### Writing and Running a Rust Program
- Rust files end with the `.rs` extension.
- Underscores are used for spaces in filenames: `hello_world.rs`.
```rust
fn main() {
    println!("Hello, world!");
}
```
#### Anatomy of a Rust Program
- Functions are defined with `fn <name>`.
- `main` is a special function, it is always run first in an executable.
- To format Rust code across file use `rustfmt`.
- Rust indents are 4 spaces, not tabs.
- Using a `!` after a name calls a [[macro]], which is not a function.
- A Rust expression is ended with a `;`,
#### Compiling and Running Are Separate Steps
- Rust code must be compiled before it can be run, to compile we run this command: `rustc <filename>`.
- Once compiled a binary executable file is created with the same name as the source file.
- To run this file we simply input the path to file, so if the executable file is in the root directory we input: `./main`.
- Unlike Python or JavaScript Rust is an [[ahead-of-time compiled language]].
### Hello, Cargo!
- [[Cargo]] is Rust's build system and [[package manager]].
- Cargo comes installed with Rust.
- To check if Cargo is installed: `cargo --version`.
#### Creating a Project with Cargo
- To create a project with Cargo: `cargo new <project name>`.
- Cargo generated two files and a directory when it creates a project:
  `Cargo.toml` and `src/main.rs`.
- It also initialises a Git repository with a `.gitignore` file.
- The [[TOML]] file is split into two sections:
  `[package]`: that declares the following statements are configuring a package.
  `[dependencies]`: lists all the dependencies for the project.
- Cargo expects source files to live in the `src` directory. 
- When in a an existing project to initialise Cargo use: `cargo init`.
#### Building and Running a Cargo Project
- To build an entire project run this command from the root directory: `cargo build`.
- This creates an executable file in `target/debug/<project name>`.
- The reason its in a `debug` folder is because the default build is a debug one.
- You can run the file by writing the file path: `./target/debug/hello_cargo`.
- When building for the first time Cargo will also create a `Cargo.lock` file.
- This files keeps track of the exact dependencies and their versions of your projects build. You will never need to change this manually.
- To compile and run source code we can use the command: `cargo run`.
- To check if code compiles use: `cargo check`.
#### Building for Release
- When a project is ready for release run: `cargo build --release`.
- This will create the executables in folder `target/release`.
#### Cargo as Convention
- While not required for simple projects Cargo's coordination becomes essential as projects grow more complex.
## Citation
---
```
S. Klabnik and C. Nichols, "Getting Started" in *The Rust Programming Language*, 2nd. J. Franklin, J. Kepler, K. Horlbeck Olsen, L. Chadwick, Eds. San Francisco, CA, USA: No Starch Press, 2023, pp. 32-47.
```


