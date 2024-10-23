##### Validating References with Lifetimes

Lifetimes are another kind of generic used in Rust. Rather than ensuring behaviour like a trait, lifetimes ensure that a reference is valid for as long as we need them to be.

This is a detail we didn't discuss in [[Rust Book - References]] is that every reference has a *lifetime*.  The lifetime is the scope for which the reference is valid. Most of the time the lifetime is implicit and inferred, the only time we annotate lifetimes is if the scope of the reference is variable. Think of it like an input where that could be multiple types. Rust requires us to annotate relationships using lifetimes so as to avoid any run time errors where a value falls unexpectedly out of scope.

##### Preventing Dangling References with Lifetimes

Lifetimes are mostly used to prevent *dangling references* which are refs that no longer point to a value and are impossible in Rust. In other languages when this occurs it can cause the program to crash.

##### The Borrow Checker

Rust has a built in borrow checker in it compiler to check for this very issue. 

##### Generic Lifetimes in Functions

In this example we'll look at a function that compares the length of two strings and returns the longest.

```rust
fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

The code above doesn't compile because the compiler can't tell if the reference returned is `x` or `y`, who have different lifetimes as they are in different branches of the conditional block.

##### Lifetime Annotation Syntax

Lifetime annotations don't change the life of a reference but they do define the relationship between lifetimes of different references without affecting each other. 

The syntax for lifetime annotations are weird, they start with an apostrophe `'`, are lowercase ad usually very short.

Most people use `'a` for the first lifetime annotation. We place this after the slice `&` of a reference.
```rust
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

##### Lifetime Annotations in Function Signatures

When using the lifetimes in function signatures we call them generic lifetime parameter and they look and function very similarly as generic type parameters.

Back to the longest string example above, we want to express the following idea: the returned reference is valid so long as both parameters are still valid.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

This function signature now reads, that for lifetime `'a` the function takes two parameters, both of whom will live at least as long as `'a`. This means that we can be certain that `'a` is essentially as long as the shortest lifetime of the values referred in the function parameters.

##### Thinking in Terms of Lifetimes

How you specify lifetime parameters depends on what the function is doing. 

```rust
fn longest<'a>(x: &'a str, y: &str) -> &'a str {
    x
}
```

In this example since we are always returning the x value then the y parameter doesn't need to be annotated with a lifetime as its lifetimes has no relationship with either x or the return value.

> [!info] 
> The return type lifetime must match at least one of the parameter lifetimes.

##### Lifetime Annotations in Struct Definitions

Structs can hold referenced as well as owned types, but to protect against dangling references we must use lifetimes to annotate the reference fields.

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}
```

##### Lifetime Elision

> elision: the omission of a sound or syllable when speaking

Rust originally required the use of writing explicit lifetimes for structs and functions for every instance and signature. Soon however they realised that lifetimes we being written in the same repetitive patterns and so to remove some redundancy they programmed the compiler to recognise these patterns so that you may omit them in most of your code. This is also interesting because as Rust usage evolves new lifetime patterns may be added to the compiler, improving the programming experience.

More [here](https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html)

##### Lifetime Annotations in Method Definitions

When implementing methods on a struct with lifetimes we use the same generic syntax as shown above.

```rust
impl<'a> ImportantExcerpt<'a> {
    fn level(&self) -> i32 {
        3
    }
}
```

##### The Static Lifetime

There is a special lifetime: `'static`.

This denotes a reference that can live for the entire duration of a program. All string literals have this lifetime.

```rust
let s: &'static str = "I have a static lifetime.";
```

##### Generic Type Parameters, Trait Bounds, and Lifetimes Together

Here is an example of applying all the generics together:

```rust
use std::fmt::Display;

fn longest_with_an_announcement<'a, T>(
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{
    println!("Announcement! {ann}");
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

