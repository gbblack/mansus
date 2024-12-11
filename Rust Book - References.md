References, like pointers, are addresses to stored data. But unlike pointers a reference is guaranteed to point to a valid value.

To denote a reference you append `&` before the type:

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{s1}' is {len}.");
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

In this example the parameter `s` needs to be of type String reference, and the variable `s1` while initialised as a `String` is *passed* as a reference using `&`.

To dereference something you use the `*` operator, [[Rust Book - Smart Pointers#Treating Smart Pointers Like Regular References with the `Deref` Trait|see more]].

When using references values do not pass ownership and as such are valid until they leave scope.

We call the action of creating a reference *borrowing*.

Borrowed values are immutable by default.

The string literal is actually a [[Rust Book - Slice Type|slice]] of the binary thus making it the immutable reference: `&str`.

##### Mutable References

To make a reference mutable we use `&mut` instead. 

Mutable references have one big difference to immutable ones and that is there can only be one mutable reference to each value at a time, *simultaneously*. This means that there can be multiple mutable references to the same value in multiple different scopes but only one per scope:

```rust
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    } // r1 goes out of scope here, so we can make a new reference with no problems.

    let r2 = &mut s;
```

This is done so rust can avoid *data races*. A data race is when at compile time these three behaviours occur:

- Two or more pointers access the same data at the same time
- At least one of the pointers is being used to write tot the data
- There is no mechanism being used to synchronize access to the data

These data races can cause unexpected behaviour and cause problems at run time.

You also cannot have a mutable reference to a value that already has an immutable reference within that same scope.



