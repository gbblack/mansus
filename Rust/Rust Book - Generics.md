### Generic Data Types
Generics are used to create definitions for items like function signatures or struts, which are then used with concrete data types. 
##### In Function Definitions
We replace the data annotation with a generic in function signatures:

```rust
// with generics
fn largest<T>(list: &[T]) -> &T

// without

fn largest_i32(list: &[i32]) -> &i32
fn largest_char(list: &[char]) -> &char
```

##### In Struct Definitions
We can also use generics within struct parameters.

```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let integer = Point { x: 5, y: 10 };
    let float = Point { x: 1.0, y: 4.0 };
}
```

In this example above we've defined that both fields use type `T`, this means that in implementation while `T` can be anything both instances of `T` must be the same kind of anything.

```rust
let wont_work = Point { x: 5, y: 4.0 };
```

This won't work since x is an `int` and y a `float`. If both we're on or the other then the code would compile. if you include another genric in the struct definition this sort of flexibility can be implemented.

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

fn main() {
    let both_integer = Point { x: 5, y: 10 };
    let both_float = Point { x: 1.0, y: 4.0 };
    let integer_and_float = Point { x: 5, y: 4.0 };
}
```

##### In Enum Functions
The same type of definition can be used for enums. Most famously this is used for the `Option<T>` enum:
```rust
enum Option<T> {
    Some(T),
    None,
}
```

With out new understanding of generics we can see now that the generic `T` of any type can be passed into Some otherwise the None value holds nothing. This allows for the expression of an optional value since Some can hold anything and None is nothing.

Enums like structs can also use multiple generics, as seen here:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```