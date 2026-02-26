---
kind: chapter
tags:
---
[[The Rust Programming Language]]
# Generic Types, Traits, and Lifetimes

> [!abstract] Summary
### Note
---
Removing Duplication by Extracting a Function
##### Generic Data Types
In Function Definitions
In Struct Definitions
In Enum Definitions
In Method Definitions
Performance of Code Using Generics
##### Traits: Defining Shared Behavior
Defining a Trait
Implementing a Trait on a Type
Default Implementations
Traits as Parameters
Returning Types That Implement Traits
Using Trait Bounds to Conditionally Implement Methods
##### Validating References with Lifetimes
Preventing Dangling References with Lifetimes
The Borrow Checker
Generic Lifetimes in Functions
Lifetime Annotations in Function Signatures
Thinking in Terms of Lifetimes
Lifetime Annotations in Struct Definitions
Lifetime Elision
Lifetime Annotations in Method Definitions
The Static Lifetime
Genetic Type Parameters, Trait Bounds, and Lifetimes Together
## Citation
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


Traits are a Rust functionality to define shared behaviour, like an interface. *Trait bounds* can be used to specify that a generic type must have certain behaviour, like it must be able to multiply, or inversely, to concatenate.

This is how you define a trait:
```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

We use a semicolon after the return type instead of `{}` as each type that implements this trait will need to define for itself what exactly occurs in the `summarize` function, think *abstract interfaces* in Java.

##### Implementing a Trait on a Type

When we got to implement a trait on a a struct it'll look a little like this:
```rust
pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
	    format!("{}, by {} ({})",
		    self.headline, 
		    self.author,
	        self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```

Like a regular method we start with the `impl` keyword and then the trait name followed by the struct.

```rust
impl Trait for Struct {
	fn traitmethod() -> type {
		// implement behaviour here
	}
}
```

> [!info] Scope
> When bringing structs that implement a trait into scope we must also bring in the trait itself into scope.
> 
> When implementing traits another scope consideration is required in that only traits local to the implementation can be used.

##### Default Implementations

Sometimes default behaviour on a trait is desired, we can define this directly within the trait block.

```rust
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}
```

When we want to use the default implementation of a trait we specify an empty block in the struct implementation.

```rust
impl Summary for NewsArticle {}
```

A default implementation is overridden by future definitions, so the previous examples still work without change because for that struct the `summarize` implementation has overwritten the original default one.

Default implementations can also call upon other methods within that trait.

```rust
pub trait Summary {
    fn summarize_author(&self) -> String;

    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}
```

In this case any struct that wants to use this trait would need only define the `summarize_author` function. Until this is defined we cannot call the default function that relies on it.

##### Traits as parameters

When we want to define a function signature by types that implement a trait we can use a reference of the trait implementation and pass that in as a type annotation in the parameters.

```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

So in the example above item can only be types that implement the `Summary` trait.

##### Trait Bound Syntax

The above is actually syntax sugar for a longer definition:

```rust
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

In most cases the difference between these two definitions is non existent however if we were to desire to have two parameters that are of the same type and implement a trait we must use the longer definition, like this:

```rust
pub fn notify<T: Summary>(item1: &T, item2: &T) {}
```

In this example both parameters are of the same type T, who implements `Summary`.

If we were not to care what type each parameter was so long as it implemented the trait we could use the simpler syntax:

```rust
pub fn notify(item1: &impl Summary, item2: &impl Summary) {}
```

##### Cleaner Trait Bounds with `where` Clauses

If we use too many trait bounds in a definition things can get messy and confusing. To solve for this Rust has a special syntax using the `where` clause. To keep things readable when a definition begins to look like this:

```rust
fn some_function<T: Display + Clone, U: Clone + Debug>(t: &T, u: &U) -> i32 {)
```

We use the `where` clause to break it up and keep it legible:

```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
where
    T: Display + Clone,
    U: Clone + Debug,
{
```

##### Returning Types that Implement Traits

The `impl` trait syntax also works when defining the return type of a function.

```rust
fn returns_summarizable() -> impl Summary {
    Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    }
}
```


This functionality is especially useful for [[Rust Book - Iterators|iterators]] and [[Rust Book - Closures|closures]] as both these features are essentially types that only the compiler knows or types with very long definitions. The `impl Trait` syntax allows you to specify that some type returns an `Iterator` trait without needing to type it out. 

The limitation is that you can only return by trait if you're also returning a single type. So in our above example both struct `NewsArticle` and `Tweet` implemented the `Summary` trait but in this return example we can only return `Tweet` not both. More info in [[Chapter 17]].

##### Using Trait Bounds to Conditionally Implement Methods

By using a trait bound on generic parameters we can implement methods conditionally for types that use that trait. Basically this means for the same trait function you can implement different behaviours depending on the input. 

```rust
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}

impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```

> For example, the type `Pair<T>` in Listing 10-15 always implements the `new` function to return a new instance of `Pair<T>` (recall from the [“Defining Methods”](https://doc.rust-lang.org/book/ch05-03-method-syntax.html#defining-methods) section of Chapter 5 that `Self` is a type alias for the type of the `impl` block, which in this case is `Pair<T>`). But in the next `impl` block, `Pair<T>` only implements the `cmp_display` method if its inner type `T` implements the `PartialOrd` trait that enables comparison _and_ the `Display` trait that enables printing.

We can also conditionally implement a trait on a. struct that implements another trait, these are called [[blanket implementations]].

More info in the rust documentation under implementors.

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

