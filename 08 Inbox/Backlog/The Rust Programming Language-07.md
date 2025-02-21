---
tags:
  - type/chapter
created:
---
[[The Rust Programming Language]]
# Managing Growing Projects with Packages, Crates, and Modules

> [!abstract] Summary
### Note
---
##### Packages and Crates
##### Defining Modules to Control Scope and Privacy
##### Paths for Referring to an Item in the Module Tree
Exposing Paths with the `pub` Keyword
Starting relative Paths with `super`
Making Structs and Enums Public
##### Bringing Paths Into Scope with the `use` Keyword
Creating Idiomatic `use` Paths
Providing New names with the `as` Keyword
Re-exporting Names with `pub use`
Using Nested Paths to Clean Up Large use Lists
The Glob Operator
##### Separating Modules Into Different Files

## Citation
---
```
S. Klabnik and C. Nichols, "Common Programming Concepts" in *The Rust Programming Language*, 2nd. J. Franklin, J. Kepler, K. Horlbeck Olsen, L. Chadwick, Eds. San Francisco, CA, USA: No Starch Press, 2023, pp. 48-?.
```

---
##### Completion Checklist
###### I. To Become Dark
- [ ] Write the Chapter title in the heading.
- [ ] Fill in the `created` property.
- [ ] Link to the book's index note.
- [ ] Complete the `Citation` section.
- [ ] Read the chapter once in its entirety with focus, no music.
- [ ] Add tag `status/dark`.
###### II. From Dark to Dawn
- [ ] Read the chapter again, this time copy pasting interesting sections into the note under `Highlights`. Include the page number right above the block.
- [ ] **Bold** the portions of the `Highlights` you find most interesting.
- [ ] ==Highlight== the best parts of the bolded sections.
- [ ] Update status tag to `status/dawn`.
###### III. From Dawn to Day
- [ ] Write the chapter `Summary`.
- [ ] Remove or complete the `Nota Bene` as necessary.
- [ ] Fill in the context tags for the metadata.
- [ ] Update status tag to `status/day`.
- [ ] Remove this checklist.

**For a technical text:**
###### II. From Dark to Dawn
- [ ] Wait at least 5mins before beginning this section.
- [ ] In the `Note` section breakdown the chapter into its subheading: all the sections in bold.
- [ ] In bullet points, under each section and sub section heading summarise the major points of that section. In these bullet point make links to anything that could be referenced in a permanent note. This will take awhile.
- [ ] While summating the sections copy paste interesting sections into the note under `Highlights`. Include the page number right above the block. These highlights should only be the author's own reflections that you think are interesting, nothing definitive. There may be nothing.
- [ ] Update status tag to `status/dawn`.
###### III. From Dawn to Day
- [ ]  **Bold** the portions of the `Highlights` you find most interesting.
- [ ] ==Highlight== the best parts of the bolded sections.
- [ ] Write the chapter `Summary`.
- [ ] Remove or complete the `Nota Bene` as necessary.
- [ ] Fill in the context tags for the metadata.
- [ ] Update status tag to `status/day`.
- [ ] Remove this checklist.

[[Cargo Workspaces]]

Rust has a *module system* to organise a project it consists of:

- **Packages**: A Cargo feature for letting you build, test and share crates
- **Crates**: A tree of modules that produce a library or executable
- **Modules** and **use**: let you control the organisation, scope and privacy of paths
- **Paths**: A way of naming an item i.e. structs, functions or modules

##### Packages and Crates

A *crate* is the smallest amount of code that the compilers considers at a time. A crate can contain modules that are defined in other files that get compiled at the same time. A crate is a file and a directory of the same name. The file holds the executable and the directory holds all the actual code and definitions that the crate is defined by, i.e. all the code in a package if this were node.

A crate can come in to forms: a binary crate or a library crate.

Binary crates are programs that compile into an executable you can run. Each crate must have a `main()` function that defines what happens when you run the crate.

Library crates are missing the `main()` function as they are not executable. They are instead used for defining functionality and are meant to be shared with multiple projects. In rust the word crate is used how package or library is used on other languages. It is code defined by someone else that you import and use for your own projects, though you can also define your own lib crates too.

There is a source file called the *crate root* that that is where the compiler starts when making the root module of your crate. More [[Rust Book - Managing Projects#Defining Modules to Control Scope and Privacy|below]]

A *package* is a bundle of one or more crates that provides a set of functionality. A package must contain a *Cargo.toml* file that describes how to build those crates. A package can contain as many binary crates as desired but only ONE library crate. Think of a package as a project.

To initialise a new package we use the command `cargo new <project name>` to create the package, which initialises the Cargo.toml file automatically. It should also create a `src` folder that contains the `main.rs` file. As the packages grows the binary crates are most often placed in a single folder called `bin` with each file being its own binary crate.
##### Defining Modules to Control Scope and Privacy

Best practice for organising rust code:

- **Start from the crate root**: This file is usually *src/lib.rs* for a library crate or *src/main.rs* for a binary crate.
- **Declaring modules**: In the crate root file you declare new modules using the `mod` keyword followed by its name -> `mod garden;`. The compiler looks for a mod statement in three places:
	- inline, within curly brackets replacing the `;` form the garden example.
	- In the file *src/garden.rs*
	- In the file *src/garden/mod.rs*
- **Declaring submodules**: In any file other than the crate root you can declare submodules. and example id declaring `mod vegetable` inside of the *src/garden.rs* file. The compiler looks for this mod's code within the directory named for the parent module, so inside the garden directory.
- **Paths to code in modules**: When a module is part of a crate its code can be used by any other file in in the crate so long as it is not private. For example if you need to access an `Asparagus` type from within the garden module this is one way -> `crate::garden::vegetable::Asparagus`
- **Private vs Public**: Code within a module is private form its parents by default, to be made public you must use the `pub mod` keywords instead of `mod`. To make other items public you place the `pub` keyword in front of them as well.
- **The `use` keyword**: Within a scope the `use` keyword creates shortcuts to reduce the path length when trying to access an item, it is like the import keyword of other languages. To access an item we do `use crate::garden::vegetable::Asparagus` and any time we want the accessed item we can just call `Asparagus`.

Here is an example of all this in action for a project called backyard:

```rust
backyard
├── Cargo.lock
├── Cargo.toml
└── src
    ├── garden
    │   └── vegetables.rs

			#[derive(Debug)]
			pub struct Asparagus {}

    ├── garden.rs

		pub mod vegetables;
		
    └── main.rs // crate root
    
		use crate::garden::vegetables::Asparagus;

		pub mod garden;
		
		fn main() {
		    let plant = Asparagus {};
		    println!("I'm growing {plant:?}!");
		}
```

##### Grouping Related Code in Modules

Modules allow us to organise code in a crate for readability and reuse. They also cover the privacy of items, with items being private by default. Making them public exposes them for external code to use.

An example for a project called restaurant:

```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}

        fn seat_at_table() {}
    }

    mod serving {
        fn take_order() {}

        fn serve_order() {}

        fn take_payment() {}
    }
```

```rust
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment
```

##### Paths for Referring to an Item in the Module Tree

In rust, like most other languages uses paths to access items across its code. Unlike most languages however that path is not the file path but more like an access path. As in you have to trace down each module name until you reach the item. Each identifier is separated by `::`.

There are two types of paths: an *absolute path* that contains every identifier from the crate root down. And a *relative path* that starts from the current module and uses `super`, `self` or an identifier in the current module.

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    // Absolute path
    crate::front_of_house::hosting::add_to_waitlist();

    // Relative path
    front_of_house::hosting::add_to_waitlist();
}
```

Think of adding the `pub` keyword like using the export in typescript, use it when you intend for this item to be used by pieces of code outside its file.

The level of privacy is most important if you want to share your crate, more information here for best practice here -> [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/).

##### Starting Relative Paths with `super`

We can use `super` to start the path in the parent module, its like using `..` in a file path.

###### Making Structs and Enums Public

Using `pub` before a struct or enum definition makes them public, but the fields will still be private. Fields are made public on a case by case basis using the `pub` keyword. If an enum is made public all of its states are also made public. 

##### Bringing Paths into Scope with `use`

You can use the `use` keyword like `import` from other languages to create a shorthand of a path so you don't need to keep rewriting it over and over again.

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

the last identifier on the use statement is what you're importing so `hosting::add_to_waitlist()` is like `package.someFunction` in other languages.

However `use` shorthand is only valid from within the scope it is brought in, so this code below doesn't compile as the `use` statement is brought inside a different scope from the one it is being called in.

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

mod customer {
    pub fn eat_at_restaurant() {
        hosting::add_to_waitlist();
    }
}
```

##### Creating Idiomatic `use` Paths

When trying to bring in structs, enums or other items that aren't modules it's best to use the full path with `use`. Such as the HashMap Struct in this code here:

```rust
use std::collections::HashMap;

fn main() {
    let mut map = HashMap::new();
    map.insert(1, 2);
}
```

The only time we don't do this is if we want to bring in two items with the same name, like here:

```rust
use std::fmt;
use std::io;

fn function1() -> fmt::Result {
    // --snip--
}

fn function2() -> io::Result<()> {
    // --snip--
}
```

This works because we don't bring in `std::fmt::Result` and `std::io::Result` because otherwise it would confuse the compiler on which one we went mean, by staying further up the path we can differentiate both `Result` when we call them.

##### Providing new names with the `as` Keyword

Another solution to two items with the same name is using `as` to give it a new name in the code.

```rust
use std::fmt::Result;
use std::io::Result as IoResult;

fn function1() -> Result {
    // --snip--
}

fn function2() -> IoResult<()> {
    // --snip--
}
```

##### Re-exporting Names with `pub use`

When a name is brought into scope with `use` the new name is private by default. If we need to access the use statement outside of its scope we *re-export* it using the `pub use` keywords. This allows for the exported item to be used outside the scope it was brought in preventing the compile error from above.

##### Using External Packages

When bringing in external packages into your project the `use pkg_name::item` format is used. This package must first be installed on the project using crates.io through the `cargo add pkg` command line call.

##### Using Nested Paths to Clean Up Large `use` Lists

If several items from one package need to brought in at same scope we can do so in this way:

```rust
use std::{cmp::Ordering, io};
use std::io::{self, Write}; // brings in io and io::Write
```

##### The Glob Operator

When we need to bring in all items from a package we use the glob `*` operator.

```rust
use std::collections::*;
```

##### Separating Modules into Different Files

As your code grows more complex you'll want to define your modules into separate files for better clarity and navigation. 

To do this define a mod with `pub mod <name>` and to call it in another file call the `pub use` or just `use` keywords. Keep the module file inside a directory that has the same name as the parent module it is a part of or the root.
