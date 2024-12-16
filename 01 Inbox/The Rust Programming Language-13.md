---
tags:
  - type/book_chapter
created:
---
[[The Rust Programming Language]]
# **Functional Language Features: Iterators and Closures**

> [!abstract] Summary
### **Note**
---
##### **Closures: Anonymous Functions that Capture Their Environment**
**Capturing the Environment with Closures**
**Closure Type Inference and Annotation**
**Capturing References of Moving Ownership**
**Moving Captured Values Out of Closures and the Fn Traits**
##### **Processing a Series of Items with Iterators**
**The Iterator Trait and the next Method**
**Methods That Consume the Iterator**
**Methods That Produce Other Iterators**
**Using Closures That Capture Their Environment**
##### **Improving Our I/O Project**
**Removing a clone Using an Iterator**
**Making Code Clearer with Iterator Adapters**
**Choosing Between Loops and Iterators**
##### **Comparing Performance: Loops vs. Iterators**
### **Highlights**
---
9 (page number)
> example highlight
##### **Citation**
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

A significant influence on Rust is [[functional programming]]. To program in a functional style we use functions as values passed into other functions, return function, assign functions to variables and more.

Rust share some of this functionality through these features:

- Closures: a function like construct to store a variable
- Iterators: a way of processing a series of elements

### Iterators

In Rust an iterator is the trait `Iterator` applied over a collection. It requires only that a method for `next` be defined.
When using the `for` construct this happens automatically as it internally uses the `.into_iter()` method.

```rust
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;

    // methods with default implementations elided
}
```

Iterators use `Self::Item` which is an [[Chapter 19|associated type]].

We can also call the next method explicitly, but it requires the variable holding it to be immutable:

```rust
#[test]
fn iterator_demonstration() {
	let v1 = vec![1, 2, 3];

	let mut v1_iter = v1.iter();

	assert_eq!(v1_iter.next(), Some(&1));
	assert_eq!(v1_iter.next(), Some(&2));
	assert_eq!(v1_iter.next(), Some(&3));
	assert_eq!(v1_iter.next(), None);
}
```

##### Methods that Consume the Iterator

The `Iterator` trait has several default implementations in the standard library. Some of them call the `next` method in their definitions which is why you need to define it in your implementation as well.

Methods that call `next` are called [[consuming adaptors]] because to call them uses up the iterator, i.e. once called whatever variable now owns that iterator and it can't be called again.

##### Methods that Produce Other Iterators

[[Iterator adaptors]] are methods defined on the `Iterator` trait that don't consume it, instead they produce a new one that shares some aspects of the original.

An example of one of these is the method `map` which takes a closure to call on each item that is iterated upon and returns a new iterator with the modified item. 

##### Using Closures that Capture Their Environment

Many [[Iterator adaptors]] take closures as arguments and can commonly be closures that capture their environment.

An example id the method `filter`: it work by a  closure getting an item from an iterator and returning a `boolean`. If the return value is `true` the value will be included in the iteration and if it returns `false` it will not be included.

A significant influence on Rust is [[functional programming]]. To program in a functional style we use functions as values passed into other functions, return function, assign functions to variables and more.

Rust share some of this functionality through these features:

- Closures: a function like construct to store a variable
- Iterators: a way of processing a series of elements

### Closures

Rust's closures are [[anonymous functions]] that can be saved into a variable or passed as an argument into another function. 

Here is an example of a closure that captures the `x` variable:

```rust
|val| val + x
```

It works similarly to arrow syntax in Typescript.

```ts
(val) => { val + x };
```

Critically both of these statement should be stored inside a variable to be of use.

Characteristics of a closure include:
- using `||` instead of `{}` around input variables
- optional body delimitation (`{}`) for a single line expression (usually mandatory)
- the ability to capture the outer environment variables

The power of closures is that you can create it in one place and then call and evaluate it elsewhere in a different context.

[more info](https://doc.rust-lang.org/rust-by-example/fn/closures.html)

##### Capturing the Environment with Closures

Closures can capture variables from within the environment they're created in, unlike functions. Imagine this scenario:

>Every so often, our t-shirt company gives away an exclusive, limited-edition shirt to someone on our mailing list as a promotion. People on the mailing list can optionally add their favorite color to their profile. If the person chosen for a free shirt has their favorite color set, they get that color shirt. If the person hasn’t specified a favorite color, they get whatever color the company currently has the most of.

One solution to this is 

```rust
#[derive(Debug, PartialEq, Copy, Clone)]
enum ShirtColor {
    Red,
    Blue,
}

struct Inventory {
    shirts: Vec<ShirtColor>,
}

impl Inventory {
    fn giveaway(&self, user_preference: Option<ShirtColor>) -> ShirtColor {
        user_preference.unwrap_or_else(|| self.most_stocked())
    }

    fn most_stocked(&self) -> ShirtColor {
        let mut num_red = 0;
        let mut num_blue = 0;

        for color in &self.shirts {
            match color {
                ShirtColor::Red => num_red += 1,
                ShirtColor::Blue => num_blue += 1,
            }
        }
        if num_red > num_blue {
            ShirtColor::Red
        } else {
            ShirtColor::Blue
        }
    }
}

fn main() {
    let store = Inventory {
        shirts: vec![ShirtColor::Blue, ShirtColor::Red, ShirtColor::Blue],
    };

    let user_pref1 = Some(ShirtColor::Red);
    let giveaway1 = store.giveaway(user_pref1);
    println!(
        "The user with preference {:?} gets {:?}",
        user_pref1, giveaway1
    );

    let user_pref2 = None;
    let giveaway2 = store.giveaway(user_pref2);
    println!(
        "The user with preference {:?} gets {:?}",
        user_pref2, giveaway2
    );
}
```

In this implementation we focused on closures, as such `store`, defined in `main`, has two blue shirt and one red. We call the `giveaway` method for both users with and without a preference.

>In the `giveaway` method, we get the user preference as a parameter of type `Option<ShirtColor>` and call the `unwrap_or_else` method on `user_preference`. The [`unwrap_or_else` method on `Option<T>`](https://doc.rust-lang.org/std/option/enum.Option.html#method.unwrap_or_else) is defined by the standard library. It takes one argument: a closure without any arguments that returns a value `T` (the same type stored in the `Some` variant of the `Option<T>`, in this case `ShirtColor`). If the `Option<T>` is the `Some` variant, `unwrap_or_else` returns the value from within the `Some`. If the `Option<T>` is the `None` variant, `unwrap_or_else` calls the closure and returns the value returned by the closure.
>
>We specify the closure expression `|| self.most_stocked()` as the argument to `unwrap_or_else`. This is a closure that takes no parameters itself (if the closure had parameters, they would appear between the two vertical bars). The body of the closure calls `self.most_stocked()`. We’re defining the closure here, and the implementation of `unwrap_or_else` will evaluate the closure later if the result is needed.

##### Closure Type Inference and Annotation

Another difference between closures and functions is that closures don't require type annotations or a return value. This is because closures are stores inside variables and are used without names which exposes them to users of the library.

In this way closures are usually short and are relevant only within a narrow context. Because of this limited context the compiler can infer the return type. 

Type annotations may be added to increase clarity and explicitness, though it may lead your code to being unnecessarily verbose. It would look like this:

```rust
let expensive_closure = |num: u32| -> u32 {
	println!("calculating slowly...");
	thread::sleep(Duration::from_secs(2));
	num
};
```

With the type annotation the closure syntax looks more similar to that of functions, here is an example of a function that adds 1 to its parameter, and its closure equivalent.

```rust
fn  add_one_v1   (x: u32) -> u32 { x + 1 }
let add_one_v2 = |x: u32| -> u32 { x + 1 };
let add_one_v3 = |x|             { x + 1 };
let add_one_v4 = |x|               x + 1  ;
```

All of the above closure definitions are valid, the only difference is the degree of explicitness and clarity in the code.

When not explicitly annotated the compiler assigns the closure the type inferred form its first use. So if a closure is used to handle strings earlier on in the program and then tries to handle an integer it will fail.

```rust
let example_closure = |x| x;

let s = example_closure(String::from("hello")); // passes
let n = example_closure(5); // fails
```

##### Capturing References or Moving Ownership

Closures can captures values in three ways, mimicking the ways a function can take a parameter, borrowing immutably/mutably and taking ownership. 

A closure can capture any value in the environment that is already in scope.

```rust
fn main() {
    let list = vec![1, 2, 3];
    println!("Before defining closure: {list:?}");

    let only_borrows = || println!("From closure: {list:?}");

    println!("Before calling closure: {list:?}");
    only_borrows();
    println!("After calling closure: {list:?}");
}
```

In this example the `only_borrows` closure will print the list value as if nothing is specified  in the body after `||` it will capture whatever is valid and in scope, in this case `list`. This is an example of an immutable capture closure.

If we change the body to include some behaviour the closure will become a mutable borrow closure.

```rust
fn main() {
    let mut list = vec![1, 2, 3];
    println!("Before defining closure: {list:?}");

    let mut borrows_mutably = || list.push(7);

    borrows_mutably();
    println!("After calling closure: {list:?}");
}
```

There's no `println!` as part of the closure in this example as you can't make an immutable borrow, which is what `println!` [[println! macro|does ]], on a mutable borrow.

If you wan the closure to take ownership of the value we need to use the `move` keyword.

##### Moving Captured Values Out of Closures and the `Fn` Traits

Once a value is moved into a closure the closure body can make changes to that value that persist when evaluated later. Depending on how the closure changes (or not) the values inside of it affects the traits the closure implements, and thus can only be called once.
A closure will implement on or all of these `Fn` traits depending on the functioning of the body:
- `FnOnce`: applies to closures that can be called once. All closure implement this trait. If a closure `moves` values out of its body this will be the only trait it implements.
- `FnMut`: applies to closures that don't move values out of its body but instead change the values. These can be called more than once.
- `Fn`: applies to closures that don't do anything to their values and to ones who capture nothing. These closures can be called many times without changing the environment they're capturing in.

[more into](https://doc.rust-lang.org/book/ch13-01-closures.html)
