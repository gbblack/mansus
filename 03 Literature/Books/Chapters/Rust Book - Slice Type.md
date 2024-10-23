
A slice lets you reference a contiguous sequence of elements in a collection and since it is a reference this slice does not have ownership.
##### String Slices

A `String slice` is a reference to a part of a string, written as:

```rust
    let s = String::from("hello world");

    let hello = &s[0..5];
    let world = &s[6..11];
```

Slices are created by indicating a range in `[]` with it written like:

```rust
[start..end] // start: inclusive, end: not inclusive
```

To start at index 0 rust understands these two ranges to be the same:

```rust
[0..2]
[..2]
```

And these to be the same:

```rust
[3..len]
[3..]
```

if you want to take a slice of the entire string you can drop both ends:

```rust
[0..len]
[..]
```

##### String Slices as Parameters

String literals are actually just slices of the binary, they are type `&str` and are an immutable reference. As such it is better practice to pass around a `&str` (string slice) rather than a `String` since as a reference ownership is not changed.

This can keep your code more generic.

##### Other Slices

A `&str` is specific to slices but a slice can be applied to any collection. We can slice an array like so:

```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];

assert_eq!(slice, &[2, 3]);
```

The slice above has the type: `&[i32]` and works the same as string slices do.