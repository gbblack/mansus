---
tags:
  - type/book_chapter
created:
---
[[The Rust Programming Language]]
# **Understanding Ownership**

> [!abstract] Summary
### **Note**
---
##### **What is Ownership?**
**The Stack anf the Heap**
**Ownership Rules**
**Variable Scope**
**The `String` Type**
**Memory and Allocation**
Variables and Data Interacting with Move
Scope and Assignment
Variables and Data Interacting with Clone
Stack-Only Data: Copy
**Ownership and Functions**
**Return Values and Scope**
##### **References and Borrowing**
**Mutable References**
**Dangling References**
**The Rules of Reference**
##### **The Slice Type**
**String Slices**
String Literals as Slices
String Slices as Parameters
**Other Slices**
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

In Rust ownership is what sets it apart. Rust does not have garbage collection what it instead has for memory management is ownership.

The ownership is defined by a set of rules the compiler checks, if any are broken the code won't compile.

### Stack and Heap

Since rust is a systems language knowing the difference between the stack and the heap is important as it will affect your code.

##### Stack

The Stack stores values in the order they come in and thus removes them in reverse, this is called *last in, first out* (think of a stack of plates).

When you add data you *push* onto the stack and when removing you *pop* off the stack.
All data on the stack must have a known fixed size on compile time.

##### Heap

The Heap is more unorganised, when string data on the heap you essentially request some space. The memory allocator then finds a some space of that size and dumps the data there, in return it gives you a pointer to that address in space. This is called *allocating*.

The pointer is a known fixed size so you can store that onto a stack, but do reach the data you must follow the pointer, think of a host seating you at a restaurant.

Pushing to stack is faster than allocating to heap because there is no search for where to allocate data, the computer just puts the data at the top of the stack every time.

When a function is called the values (including pointers) inside get pushed onto the stack, when the function ends they get popped off.

Ownership is the strategy rust uses to manage the Stack and Heap in an efficient and safe manner.

### Ownership Rules

1. Each value has an owner
2. There can only be one owner at a time
3. When the owner goes out of scope, the value will be dropped

##### Variable Scope

variables are only available within the scope in which they are declared.

```rust
    {                      // s is not valid here, it’s not yet declared
        let s = "hello";   // s is valid from this point forward

        // do stuff with s
    }                      // this scope is now over, and s is no longer valid

```

When `s` comes into scope it is *valid*
When `s` comes out of scope it becomes *invalid*

##### String Type

The strings come in different flavours. There is the string literal which is what's used above. But this string is immutable and there are occasion, say user input, where we won't know already the size of the string. [[Rust Book - String]]

There is also the `String` types, which is data allocated to the heap. Written as:

```rust
let s = String::from("hello");
```

The `::` allows us to namespace the `from` function under the `String` type rather than using some name like `string_from`. More in [[Method Syntax]].

This kind of string can be mutated:

```rust
    let mut s = String::from("hello");

    s.push_str(", world!"); // push_str() appends a literal to a String

    println!("{s}"); // This will print `hello, world!`

```

The difference here is that a string literal is of a know size and can be accounted for at compile time. This makes it fast and efficient.
But the `String` type is allocated to heap to support a piece of text that can grow and change. Therefore the memory must be allocated at runtime, where Rust differs is in how it restores that memory when no longer in use.

In Rust the memory is restored, or returned, automatically when the variable that *owns* the data goes out of scope.

```rust
    {
        let s = String::from("hello"); // s is valid from this point forward

        // do stuff with s
    }                                  // this scope is now over, and s is no
                                       // longer valid
```

When a variable goes out of scope rust calls a special function for us automatically `drop` which deallocates the memory.

##### Variables and Data Interacting with Move

When using simple scalar values moving data like this is intuitive:

```rust
    let x = 5;
    let y = x;
```

x is 5 and y is 5 because the value of 5 isfixed and knowable.

In this case:

```rust
    let s1 = String::from("hello");
    let s2 = s1;
```

`s1` and `s2` are not the same, since we are using the `String` type `s1` actually stores a pointer to an address on the heap that holds the data `"hello"`. `s2` also holds a pointer to that same address but these pointers are not equal.

Since `s1` and `s2` are pointing to the same address on the heap and rust automatically drops data once a variable is out of scope this has the potential of creating a memory error. Because once either variable falls out of scope, the data is deleted, even if the other variable is still in scope and needs access to that data.

To solve this, the moment `let s2 = s1` is called the variable `s1` is no longer valid, i.e. the owenership of that data passes from `s1` to `s2`.
This is known as a move. In this example `s1` was moved into `s2`.

##### Variables and Data Interacting with Clone

If there is the case where we want to deeply copy data that is on the heap we can use the special function `clone`.

```rust
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {s1}, s2 = {s2}");
```
This creates exact copy of the data on a different address in the heap, as such it can be quite expensive memory and time wise.

##### Stack Only Data: Copy

When dealing with types that are stored on the stack, such as integers, there is no need for a deep copy since the size of the data i know immidiately as such all copies are shallow copies and dat does not need to be moved around. We can use the `Copy` annotation for making copies of any trivial type.
If a type has the `Drop` annotation then it cannot be copied in this way.

### Ownership and Functions

Passing or moving values through functions work very similarly as with variables:

```rust
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it's okay to still
                                    // use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{some_string}");
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{some_integer}");
} // Here, some_integer goes out of scope. Nothing special happens
```

Once the function `takes_ownership` is called `s` is no longer valid and cannot be called upon again. 

##### Return Values and Scope

Return values can also transfer ownership as seen here:

```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 was moved, so nothing
  // happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it

    let some_string = String::from("yours"); // some_string comes into scope

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function
}

// This function takes a String and returns one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                                      // scope

    a_string  // a_string is returned and moves out to the calling function
}
```

ownership works the same every time, assigning a value to another variable moves it, including assigning to a function, when a variable with data stored in the heap goes out of scope, that data is dropped and the memory restored.

This can be tedious for reusing data.

To solve this rust uses [[Rust Book - References|references]] to use a value without passing ownership.

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