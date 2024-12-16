---
tags:
  - type/chapter
created:
---
[[The Rust Programming Language]]
# **Smart Pointers**

> [!abstract] Summary
### **Note**
---
##### **Using `Box<T>` to Point to Data on the Heap**
**Using `Box<T>` to Store Data on the Heap**
**Enabling Recursive Types with Boxes**
##### **Treating Smart Pointers Like regular References with the `Deref` Trait**
**Following the Pointer to the Value**
**Using `Box<T>` Like a Reference**
**Defining Our Own Smart Pointer**
**Implementing the `Deref` Trait**
**Implicit Deref Coercions with Functions and Methods**
##### **Running Code on Cleanup with the `Drop` Trait**
##### **`Rc<T>`, the Reference Counted Smart Pointer**
**Using `Rc<T>` to Share Data**
**Cloning an `Rc<T>` Increases the Reference Count**
##### **`RefCell<T>` and the Interior Mutability Pattern**
**Enforcing Borrowing Rules at Runtime with `RefCell<T>`**
**Interior Mutability: A Mutable Borrow to and Immutable Value**
**Allowing Multiple Owners of Mutable Data with `Rc<T>` and `RefCell<T>`**
##### **Reference Cycles Can Leak Memory**
**Creating a Reference Cycle**
**Preventing Reference Cycles Using `Weak<T>`**
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

A [[pointer]] is a programming concept of a variable that stores an address in memory to a value rather than the value itself. In Rust this expresses itself mostly as a [[Rust Book - References|reference]]. A simple pointer like behaves just the same as a traditional variable.

*Smart pointers* however are a kind of [[data structure]] that act like a pointer but also have additional capabilities. Smart pointers are an idea that originated in C++ and have been picked up by multiple languages since, including Rust. In the standard library Rust has several smart pointers defined. 

Rust implements smart pointer usually in [[Rust Book - Structs|structs]]. Unlike a normal struct, a smart pointer implements the `Deref` and `Drop` traits. The first allows instances of the struct to behave like a reference, the second allows for the customisation of code that's run when the struct goes out of scope.

Since smart pointer patterns are very popular and useful many libraries write their own smart pointers but the most common you'll use form the standard library are these three:
- `Box<T>`: for allocating values on the heap
- `Rc<T>`: a reference counting type that enables multiple ownership
- `Ref<T>` and `RefMut<T>`, accessed through `RefCell<T>`: a type that enforces borrowing rules at runtime instead of compile time.
##### Using `Box<T>` to Point to Data on the Heap

The most basic smart pointer is a box, written `Box<T>`. Boxes allow for data to be stored on the heap rather than the stack. What stays on the stack is the pointer (address) to the [[Rust Book - Ownership#Heap|heap]] data. 

There is no performance overhead for boxes and as such aren't used much differently as values stored on the stack. They are most often used in these cases:
- A type with an unknown size that is in a context where exact size is needed
- When large amounts of data need to transfer ownership
- When you want to own a value where you only care about the implemented traits and not the type itself

The first use case can be seen here in this section.

The second case works because if large data needs to change ownership a lot it is cheaper and faster to instead pass the pointer throughout the stack then all the data itself as ownership changes.

The third case is known as a *trait object* as seen [[Chapter 17|here]].

##### Using a `Box<T>` to Store Data on the Heap

First, let's cover syntax, here is how to use a box to store an int value on the heap.

```rust
fn main() {
    let b = Box::new(5);
    println!("b = {b}");
}
```

We define variable `b` to have the value `Box` that points to value `5` which is sitting on the heap. When accessing `b` in the next line the program will print `b = 5`. In this example the data is accessed the same as it were on the stack as normal. When `b` goes out of scope and deallocation occurs it will happen to both the box on the stack and the value on the heap.

##### Enabling Recursive Types with Boxes

A value of *recursive type* can have another value of the same type as part of itself. This can cause a problem because Rust needs to know at compile time what size the type will be however a recursive type could potentially reoccur infinitely. But a box has a known size sof one is used in the recursive definition Rust will let it compile.

#### Cons List

A *cons list* is a data structure that comes from the Lisp programming language and its dialects and is made up of nested pairs, it is the Lisp version of a linked list. Its name comes form the `cons` function ,which is short for `construct function`, that constructs a new pair from its two arguments. By calling `cons` on a pair consisting of a value and another pair, we can construct cons lists made up of recursive pairs.

Here is a pseudocode example: `(1, (2, (3, Nil)))`.

Each item in a cons list contains two elements: the value of the current item and the next item.

The last item in the list only has a value called `Nil` as there is no next item. A cons list is produced recursively calling the `cons` function. 

In Rust a cons list isn't often used, most of the time `Vec<T>` is a better choice. But a cons list is a good example of what Rust can do with `Boz<T>`. 

##### Computing the Size of a Non Recursive Type

In the case of an [[Rust Book - Enums|enum]] Rust allocates space to the option that needs the most, since the compiler knows only one will be used if the biggest space is allocated then all the smaller options will also fit. 

In a recursive type there is no way to know how much space to allocate in that way, however if we use a box, which is a fixed size, to hold the address (pointer) of the data we want to access we can both know what size to allocate (the box) and have varying to infinite data stored (the value on the heap).

##### Treating Smart Pointers Like Regular References with the `Deref` Trait

Implementing the `Deref` trait allows you to customise the behaviour of the dereference operator, `*`.

With regular references the `*` operator is used as a way to bypass the reference and go directly to the value.

```rust
fn main() {
    let x = 5;
    let y = &x;
    
	// without * it would be comparing ref to value which is not equal
    assert_eq!(5, x); 
    assert_eq!(5, *y); 
}
```

We can change this code to use a smart pointer like `Box<T>` instead of a reference and `*` will work in the exact same way. 

[[more notes later needed]]

source: [rust book chapter 15.2-15.6](https://doc.rust-lang.org/book/ch15-02-deref.html)

