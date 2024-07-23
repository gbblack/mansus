Traits are a Rust functionality to define shared behaviour, like an interface. *Trait bounds* can be used to specify that a generic type must have certain behaviour, like it must be able to multiply, or inversely, to concatenate.

This is how you define a trait:
```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

We use a semicolon after the return type instead of `{}` as each type that implements this trait will need to define for itself what exactly occurs in the `summarize` function, think *abstract interfaces* in Java.