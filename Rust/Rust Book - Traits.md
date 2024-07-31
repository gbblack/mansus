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