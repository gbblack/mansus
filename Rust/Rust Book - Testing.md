In Rust test are functions that verify that non test code is functioning as expected. These are essentially unit tests and use the AAA framework:
- Assign: set up all needed data and state
- Action: run the code that is being tested
- Assert: check against an expected standard

##### The Anatomy of a Test Function

A test in Rust is a function with the `test` attribute. [[Attributes]] are metadata about pieces of Rust code, to make a function a test function we write `#[test]` on the line before. To run these test functions the command `cargo test` is used.

When we generate a new module in Rust a test module with a test function is also generated for us. 

To assert a function we use the `assert_eq!` [[macro]].

For documentation on test see [[Chapter 14]]

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

