---
tags:
  - type/chapter
created: 2025-01-06
---
[[The Rust Programming Language]]
# **Using Structs to Structure Related Data**

> [!abstract] Summary
## **Note**
---
- A `struct` (structure) is a custom data type that allows you to package and name multiple values into a meaningful group.
- Structs are similar to a `type` from a language like TypeScript.
### **Defining and Instantiating Structs**
- Structs are similar to tuples in that they can package multiple value types into one structure but unlike a tuple these values can be given names.
- This makes structs more flexible than tuples as you don't need to rely on order alone to access the values, you can use their names.
- To define a struct: `struct <name> { active: bool, age: i32, email: String }`.
- Once defined a struct must be instantiated. This is done by creating an *instance* of a struct with each of its fields given a concrete value.
- Written as: `let user = User { active: true, age: 12, email: "example@gamil.com };`.
- To access a value from an instantiated struct we can use dot notation: `user.age -> 12`.
- If an instance is mutable we can also change the value with this notation: `user.age = 15;`.
- The entire instance of the struct must be mutable for this, Rust does not allow for mutability in only some fields and not others.
##### **Using the Field Init Shorthand**
##### **Creating Instances from Other Instances with Struct Update Syntax**
##### **Using Tuple Structs Without Named Fields to Create Different Types**
##### **Unit-Like Structs Without Any Fields**
### **An Example Program Using Structs**
##### **Refactoring with Tuples**
##### **Refactoring with Structs: Adding More Meaning**
##### **Adding Useful Functionality with Derived Traits**
### **Method Syntax**
##### **Defining Methods**
##### **Methods with More Parameters**
##### **Associated Functions**
##### **Multiple `impl` Blocks**
###### **Citation**
---
```
S. Klabnik and C. Nichols, "Using Structs to Structure Related Data" in *The Rust Programming Language*, 2nd. J. Franklin, J. Kepler, K. Horlbeck Olsen, L. Chadwick, Eds. San Francisco, CA, USA: No Starch Press, 2023, pp. 48-?.
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


Since structs work similarly to types when used in a function we can define a parameter or return type as being an instance of a struct.

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username: username,
        email: email,
        sign_in_count: 1,
    }
}
```

if a function parameter and field name are the same you can use this shorthand:

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

in this case username and email are both fields and parameters but rust will know to pass the parameter values into the same name field.

##### Creating Instances from Other Instances with Struct Update Syntax

Often you'll need to create a new instance of a struct that carries over some data from an old one. This is done by creating a new instance and only specifying the new/updated values while using the `..` operator to denote all else is the same as a previous struct.

```rust
fn main() {
    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}
```

The new information must come before the old if using this syntax.

When using structs data is moving ownership not referencing. If a struct contains non trivial types, such as String or another struct then all of these complex types must be moved to keep the original struct valid. In this case user1 is invalid as only email was updated. If both email and username were updated user1 would still be valid since its other fields are simple data types and are thus stored on the stack and have the `Copy` trait, i.e. a new identical copy of user1 while be made with new ownership containing all the old data that has been copied over, which os not possible for types stored on the heap.

##### Using Tuple Structs Without Named Fields to Create Different Types

There are a special kind of struct called *tuple structs*, tuple structs have the added meaning of a name for the structure but no names for the fields.
These are best used when you want a name for the tuple but giving names to the fields would be too redundant.

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

You can use index notation to access individual values: `black[0] // will give you 0`.

##### Unit-Like Structs Without Any Fields

A struct without a field is called a *unit like struct*. These are useful for when you need to apply a [[Rust Book - Traits|trait]] to some type but do not know the data you'd like top store. 

```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

Since the struct is empty there's no need for `{}`.

##### Ownership of Struct Data

We want structs to own all of their data so it is best to use `String` over `&str` for the fields as it keeps the ownership belonging to each struct instance. If you try to give a field the type string slice you'll get a compiler error because you have not specified a [[Rust Book - Lifetimes|lifetime]].

When using structs that you plan to reuse pass a reference to the struct into functions and so that the function that needs to use the struct can keep its ownership.

Methods are declared with the `fn` keyword and a name. They optionally can have parameters, a return type and a body with some code.

Methods are different from functions in that they are defined within a struct, enum or trait object. They are also different in that their first parameter must always be `self`.

`self` is the instance of the struct the method is being called on.

Think of methods as behaviours the struct should be able to do, or methods in a class.
##### Defining Methods

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```

When defining a method we first use the keyword `impl` followed by the struct name, every method defined in the `impl` block will they be associated with the same name struct. 

When using the `self` parameter use it as you would if you were referencing the struct itself, because you are. As such is best to borrow self so that ownership stays with the referenced struct.

Now when you want to use the method you can use dot notation just like with fields, only you include the parameters (if any), the self parameter is implicit and does not get explicitly passed through. 

The main purpose of methods is for organisation, this way we can keep all information and behaviour of a struct in one place.

We can also use these methods for traditional getter and setter functions as methods can have the same name as fields. 

```rust
impl Rectangle {
    fn width(&self) -> bool {
        self.width > 0
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    if rect1.width() {
        println!("The rectangle has a nonzero width; it is {}", rect1.width);
    }
}
```

##### Associated Functions

All methods defined in an `impl` block are called *associated functions* because they are associated with a struct. We can have associated functions that are NOT methods (they do not need an instance of a struct), as they do not have the `self` parameter.

These functions are used mostly for constructors that will return a new instance of a struct. Convention is to call them `new` but that is not required and new is NOT a keyword in rust.

An example for the rectangle struct constructing a square (so we don't need to repeat the parameter twice).

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}
```

The `Self` keyword are aliases for the struct named in the `impl` block.

To call an associated function we use the `::` syntax with the struct name `let sq = Rectangle::square(3);` as an example. This syntax is also used for namespaces created bny [[modules]].

##### Multiple `impl` Blocks

Every struct is allowed many `impl` blocks, where this is useful is in [[Rust Book - Generics|Chapter 10 of the Rust Book]].
