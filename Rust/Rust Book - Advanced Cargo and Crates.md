Complete documentation: [cargo](https://doc.rust-lang.org/cargo/)

##### Customizing Builds with Release Profiles

In Rust [[release profiles]] are predefined and customizable profiles with different configurations that allow a programmer to have more control over various options compiling code. Each profile is independently configured.

Cargo has two main profiles: the `dev` profile for when `cargo build` is run and the `release` profile when you run `cargo build --release`. The `dev` profile has good defaults for development and the `release` profile has good defaults for release builds.

If you want to change or update these default configurations we can do that by updating the `[profile.*]` section in the *Cargo.toml* file. This way we can override to add any values to the default. 

```rust
[profile.dev]
opt-level = 0

[profile.release]
opt-level = 3
```

The `opt-level` setting controls the number of optimizations Rust applies, ranged form 0 to 3. More optimizations makes for a longer compile time so the default `dev` is set to 0 to keep it form slowing down. For `release` that value is 3. To override this setting just change the value. 

[[TBC]]