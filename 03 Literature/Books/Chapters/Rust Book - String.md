When referring to strings in rust it is either `String` or `&str`.

Many operation used on vectors can be used on `String` because it's really a wrapper around a vector of bytes.

```rust
let mut s = String::new();
```

This creates a new empty string. To make a string literal (`"something"`) a `String` we can call the `.to_string()` method on the literal. The function `String::from(literal)` also works.

