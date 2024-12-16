---
tags:
  - type/chapter
created:
---
[[The Rust Programming Language]]
# **Writing Automated Tests**

> [!abstract] Summary
### **Note**
---
##### **How to Write Tests**
**The Anatomy of a Test Function**
**Checking Results with the `assert!` Macro**
**Testing Equality with the `assert_eq!` and `assert_ne!` Macros**
**Adding Custom Failure Messages**
**Checking for Panics with `should_panic`**
**Using `Result<T, E>` in Tests**
##### **Controlling How Tests Are Run**
**Running Tests in Parallel or Consecutively**
**Showing Function Output**
**Running a Subset of Tests by Name**
**Ignoring Some Tests Unless Specifically Requested**
##### **Test Organization**
**Unit Tests**
**Integration Tests**
### **Highlights**
---
9 (page number)
> example highlight
##### **Citation**
---
```
S. Klabnik and C. Nichols, "Common Programming Concepts" in *The Rust Programming Language*, 2nd. J. Franklin, J. Kepler, K. Horlbeck Olsen, L. Chadwick, Eds. San Francisco, CA, USA: No Starch Press, 2023, pp. 48-?.
```

> [!note] Nota Bene

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

In Rust test are functions that verify that non test code is functioning as expected. These are essentially unit tests and use the AAA framework:
- Assign: set up all needed data and state
- Action: run the code that is being tested
- Assert: check against an expected standard

##### The Anatomy of a Test Function

A test in Rust is a function with the `test` attribute. [[Attributes]] are metadata about pieces of Rust code, to make a function a test function we write `#[test]` on the line before. To run these test functions the command `cargo test` is used.

When we generate a new module in Rust a test module with a test function is also generated for us. 

To assert a function we use the `assert_eq!` [[macros]].

For documentation on test see [[Rust Book - Advanced Cargo and Crates]]

##### Checking Results with the `assert!` Macro

test macros:
- `assert!` -> for asserting boolean values
- `assert_eq!` -> for asserting equality
- `assert_ne!` -> for asserting inequality

You can add custom failure messages using the `format!` macro.

If we need to check for panics we add the `#[should_panic]` attribute above the function.

To handle test results we can use `Result<T, E>` where `E` is returned instead of the program panicking. This mutually exclusive form using the `#[should_panic]` attribute.

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() -> Result<(), String> {
        if 2 + 2 == 4 {
            Ok(())
        } else {
            Err(String::from("two plus two does not equal four"))
        }
    }
}
```

### Controlling How Tests Are Run

To compile and run test code we use `cargo test`. The default behaviour is to run all of the tests after compile. There are some command line args that can be applied to either `cargo test` or the binary itself, to separate the two applications we use `--`.

Run `cargo test --help`: for info on options for running
Run `cargo test -- --help`: for info on options for binary

##### Running Tests in Parallel or Consecutively

By default multiple tests are run in parallel, to do this you must make sure thats tests don't depend on any shared state or environment variables.

If you want more control over the test threads you can use the `--test-threads` flag and set it to one to have the tests run consecutively.

```rust
cargo test -- --test-threads=1
```

##### Showing Function Output

By default if a test passes in Rust anything printed gets captured. For example if `println!` is used in a test that passes we won't see it printed in the terminal. If the test fails we will see that message along with the failure message.

If you want to see the printed value for a successful test we can use the `--show-output` flag.

```rust
$ cargo test -- --show-output
```

##### Running a Subset of Tests by Name

If you want to run only some of your tests in a suite one way to filter the running tests is by name. So if you have a test function named `one_hundred` and you only wanted to run this one test you can call: `cargo test one_hundred`.

If you want to run tests by a shared word in the name you can do this too, so if we have tests `add_one`, `add_two` and `one_hundred` we can run the first two by running: `cargo test add`. 

You can also add the `#[ignore` attribute above the test function to ignore a test until removed. We can also tun ignored tests with the `-- --ignored` flag.

To run all tests, ignored or otherwise we call: `cargo test -- --include-ignored`.

### Test Organization

In Rust tests are split into two main categories: [[Unit]] and [[Integration]]. Unit tests are small and should cover one function in isolation, they should also cover private interfaces.

Integration tests should replicate the way an external service would use your code, i.e. using only the public interfaces and exercising multiple functions at once.

##### Unit Tests

The purpose of a Unit test is to test each unit of code in isolation to help pin point any issues in logic or application. These tests should be in the `src` directory in each file with the code you're testing. The convention is to create a module `tests` in each file to contain the test functions and to annotate the module with `cfg(test)`.

##### The Tests Module and `#[cgf(test)]`

The `#[cfg(test)]` annotation on the tests module tells Rust to compile and run the test code only when you run `cargo test` and not to build it with `cargo build`. This saves compile time and space. 

Integration tests do not need this annotation as they are in a separate directory from the source code being built.

The attribute `cfg` stands for *configuration* in Rust.

##### Testing Private Functions

While in some languages it is either extremely difficult ot impossible to test private functions, in Rust we can. 

Since tests are simply Rust code and the `tests` module is just another module then items in a child module can use the items in their ancestor module, as discussed in [[Rust Book - Managing Projects#Paths for Referring to an Item in the Module Tree]]. To bring all functions into scope for the `tests` module we simply call `use super::*`.

### Integration Tests

In Rust Integration tests are entirely external to your library. They should use your library in the same way any other code would. This means they can only call upon functions that are apart of the public scope. 

For integration tests we first create a `tests` directory outside of the `src` one.

```txt
adder
├── Cargo.lock
├── Cargo.toml
├── src
│   └── lib.rs
└── tests
    └── integration_test.rs
```

Inside the `integration_test` file the test function should look something like this.

```rust
use adder::add_two;

#[test]
fn it_adds_two() {
    assert_eq!(4, add_two(2));
}
```

Each file in `tests` is a separate crate so the source code must be brought into scope each time. As such we also don't need to use `#[cfg(test)]` since it is not part of the crate build.

`cargo test` includes these test files as well when running tests for the whole crate.

If we want to run a specific integration test we add the `--test` argument before the integration filename.

##### Submodules in Integration Tests

If we want integration to share some functionality, such as setup and tear down, then we have to store all the files holding those functions in a subfolder *common* and name the file *mod.rs*.

```txt
├── Cargo.lock
├── Cargo.toml
├── src
│   └── lib.rs
└── tests
    ├── common
    │   └── mod.rs
    └── integration_test.rs
```

We bring this common module into scope using `mod common` and we call `common::<function>` to use one of the common functions defined within.

##### Integration tests for Binary Crates

Only library crates expose functions that other crates can use so your integration crates can never cover code from a binary crate. 

>This is one of the reasons Rust projects that provide a binary have a straightforward src/main.rs file that calls logic that lives in the src/lib.rs file. Using that structure, integration tests can test the library crate with use to make the important functionality available. If the important functionality works, the small amount of code in the src/main.rs file will work as well, and that small amount of code doesn’t need to be tested.
> [more understanding required](https://doc.rust-lang.org/book/ch11-03-test-organization.html)

