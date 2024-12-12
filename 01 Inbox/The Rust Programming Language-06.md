---
tags:
  - type/book_chapter
created:
---
[[The Rust Programming Language]]
# **Enums and Pattern Matching**

> [!abstract] Summary
### **Note**
---
##### **Defining an Enum**
**Enum values**
**The Option Enum and Its Advantages Over Null Values**
##### **The `match` Control Flow Construct**
**Patterns That bind to Values**
**Matching with `Option<T>`**
**Matches Are Exhaustive**
**Catch-All Patterns and the `_` Placeholder**
##### **Concise Control Flow with `if let`**
### **Highlights**
---
9 (page number)
> example highlight
##### **Citation**
---
```
First Name Initial. Last Name, "Chapter Title" in *Book Title*, edition. First Name Initial. Last Name (for all editors), Ed(s). City, (US State Only), Country: Publication, Year, pp. start page-end page.
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


Enums also known as *enumerations* are a type defined by enumerating its possible variants. Think of it like a set of states where only one state can be assigned at one time.

##### Defining an Enum

Enums are defined using the `enum` keyword along with a name and curly brackets.

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

To create an instance of an enum type we use `::` syntax.

```rust
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;
```

Enums can also include type information to keep things more concise.

```rust
enum IpAddr {
	V4(String),
	V6(String),
}

let home = IpAddr::V4(String::from("127.0.0.1"));

let loopback = IpAddr::V6(String::from("::1"));
```

As you can see above as part of the implementation of `enum` each state becomes a function that constructs its own instance. 

Enums states can also have varying types and associated data while still being a part of the same `enum` block.

```rust
enum IpAddr {
	V4(u8, u8, u8, u8),
	V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);

let loopback = IpAddr::V6(String::from("::1"));
```

Here's a more complicated example

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

This is equivalent to writing out 4 different structs, one for each state. The choice between one or the other is a design choice the the types are nearly entirely interchangeable.

You can also use the `impl` block to define methods for an `enum`.

```rust
impl Message {
	fn call(&self) {
		// method body would be defined here
	}
}

let m = Message::Write(String::from("hello"));
m.call();
```

##### The `Option` Enum And Its Advantages Over Null Values

In rust there is no `null` type. Instead we have the enum `Option`.  The Option enum defined only two states, a value can be something or it is nothing. 

Doing things this way means that all states are accounted for, including the nothing state, so if you function doesn't handle that possibility the compiler will pick it up and notify you. 

```rust
enum Option<T> {
    None,
    Some(T),
}
```

The `<T>` syntax means that the parameter is generic, i.e. it can be anything.

If you want a value to be null you have to explicitly use the Option enum.

Rust has a flow construct called `match` similar to the switch construct found in Javascript.

This construct compares values against a series of patterns and execute the code that has the matching pattern.

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

So in this example if Coin matches the Penny type return 1, 5 for Nickel and so on.

N.B. if you want to write multiples line in each arm of the match you must use `{}`.

##### Patterns That Bind To Values

We can also use match patterns with variables using typed enums.

```rust
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {state:?}!");
            25
        }
    }
}
```

##### Matches are Exhaustive

The arms if a `match` block must cover all possible states otherwise it will not compile.

Since all states must be exhaustive we can use the `other` state as a sort of default state wherein the variable matched none of the arms therefore the other arm is run.

Like in this example where if a player rolls a 3 or a 7 two different arms get executed, but in all other cases of the die roll the `other` arm is executed.

```rust
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	other => move_player(other),
}

fn add_fancy_hat() {}
fn remove_fancy_hat() {}
fn move_player(num_spaces: u8) {}
```

In the case we want a catch all value but don't want to use it we use `_` to mark the arm.

```rust
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	_ => reroll(),
}

fn add_fancy_hat() {}
fn remove_fancy_hat() {}
fn reroll() {}
```

What this does is keep the the match block going until it hits an arm that we have defined.

If we want an arm to do nothing when its executed we can use the [[Rust Book - Data Types#Tuple Type|unit tuple]] to do nothing.

```rust
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	_ => (),
}

fn add_fancy_hat() {}
fn remove_fancy_hat() {}
```

More in [[Chapter 18]]

##### `if let` Control Flow

The `if let` syntax combines the keywords `if` and `let` into one more concise expression to match one pattern and ignore the rest. Using `if let` we can simplify this code block.

```rust
let config_max = Some(3u8);
match config_max {
	Some(max) => println!("The maximum is configured to be {max}"),
	_ => (),
}
```

Into this one

```rust
let config_max = Some(3u8);
if let Some(max) = config_max { 
	println!("The maximum is configured to be {max}");
}
```

In the original block we check to see if the variable matches the Some condition but to be exhaustive we must include the `_` catch all state, otherwise rust wouldn't compile.

The `if let` syntax covers for that catch all condition, it says if condition is true let value be assigned, therefore no match statement is needed. 