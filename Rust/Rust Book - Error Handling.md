In rust there are two types or errors: *recoverable* and *unrecoverable*. To handle these two errors Rust does not use exceptions it instead uses the type `Result<T, E>` for recoverable errors and `panic!` [[macros]] for unrecoverable errors. 

### Unrecoverable Errors

There are two ways to cause a `panic!` error. The first is by taking an action that causes the code to panic, such as accessing an array index that is out of bounds, or by explicitly calling `panic!`. The default behaviour of panic is to print a failure message, clean up the stack and quit. You can also config through environment variables were Rust will display the call stack to better report the bug.

> [!info] Unwinding or aborting in response to a panic
> When the Rust cleans up the stack when a panic occurs this is called **unwinding**. This occurs by default but can take time and be a lot of work. Rust gives you the option to *abort* it instead which ends the program without cleaning it.
> Any memory that the program was using will then need to be cleaned up by the OS. To switch from the default behaviour to aborting you add `panic = 'abort'` to the appropriate `[profile]` sections in the *Cargo.toml* file. This can be done on a mode by mode basis

To call panic directly we do something like this:

```rust
fn main() {
    panic!("crash and burn");
}
```

Mostly though we will be encountering panic when it is triggered in someone else's code. To backtrace the call stack and see exactly where and what failed we can call `RUST_BACKTRACE` after the program quits: `RUST_BACKTRACE=1 cargo run`. The only requirement is that it is set to anything that is not 0.