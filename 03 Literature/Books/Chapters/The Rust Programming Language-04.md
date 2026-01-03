---
kind: chapter
tags:
---
[[The Rust Programming Language]]
# Understanding Ownership

> [!abstract] Summary
> The basic principles of ownership, references and slices in Rust.
## Note
---

> [!tip]- Table of Contents
> 1. [[#What is Ownership?]]
> 	1. [[#The Stack and the Heap]]
> 	2. [[#Ownership Rules]]
> 	3. [[#Variable Scope]]
> 	4. [[#The `String` Type]]
> 	5. [[#Memory and Allocation]]
> 		1. [[#Scope and Assignment]]
> 		2. [[#Variables and Data Interacting with Move]]
> 		3. [[#Stack-Only Data Copy]]
> 	6. [[#Ownership and Functions]]
> 		4. [[#Return Values and Scope]]
> 2. [[#References and Borrowing]]
> 	1. [[#Mutable References]]
> 	2. [[#Dangling References]]
> 	3. [[#The Rules of Reference]]
> 3. [[#The Slice Type]]
> 	1. [[#String Slices]]
> 		1. [[#String Literals as Slices]]
> 		2. [[#String Slices as Parameters]]
> 	2. [[#Other Slices]]
### What is Ownership?
- [[Ownership]] is the feature that sets Rust apart. It is what allows for memory safety without garbage collection.
- Ownership is the set of rules Rust uses to govern memory. If any of these rules are broken the code won't compile, which is how Rust can have memory safety.
#### The Stack and the Heap
- In Rust understanding the difference between how memory is allocated on the stack vs. the heap is essential as Rust is a systems language.
- Both the stack and heap store memory for code to access at runtime. The difference lies in how they are structured.
- The stack stores values in the order it receives them, and returns them in the opposite order, this is known as *last in first out*.
- Think of the stack as a stack of plates, you both place and grab off the top.
- Adding to the stack is called *pushing* and removing from the stack is *popping*.
- All data on the stack must have a known, fixed size.
- The heap is what it sounds like, a heap of space. When allocating memory to the heap the allocator finds and empty spot big enough to hold the data, marks it as in use and then returns a [[pointer]] which has the address of the location in memory.
- Think of a server at a restaurant guiding your party to a table.
- Pushing onto the stack is faster than allocating to the heap, since the allocator must search for space whereas the stack just adds the data to the top.
- Accessing the data on the stack is faster than the heap too because there's no extra step of following the pointer.
- When code calls a function all the values passed into it, including pointers, get pushed onto the stack. When the function is over they get popped off.
- Ownership addresses all the heap issues such as: keeping track of what code references what address, cleaning up unused data, and minimising duplication.
#### Ownership Rules
- There are 3 rules in ownership:
	1. Each value in Rust has an *owner*.
	2. There can only be one owner at a time.
	3. When the owner goes out of scope, the value will be dropped.
#### Variable Scope
- To understand ownership we need to understand [[scope]], which is the range within a program for which an item is valid.
- Once a variable comes into scope, it is valid. When it goes out of scope the value is dropped and the variable becomes invalid.
#### The `String` Type
- The primitive data types are all handled on the stack, as they are always of a know fixed size.
- The `String` is a Rust type that is stored on the heap.
- `String` is different from a string literal (`"example"`) as the literal is immutable.
- To create a `String`from a literal we use the `from` function, `String::from("hello");`.
- The double colon `::` notations allows us to namespace the `from` function under the `String` type rather than using a name like `string_from`.
- The `String` type can be mutated.
#### Memory and Allocation
- The reason `String` is mutable whereas a string literal is not it because the literal is hardcoded directly into memory (stack) and is therefore of a know size. The `String` type instead is allocated onto the heap as its size is unknown at compile time.
- In most languages a garbage collector is used to clear up unused memory on the heap, which are difficult to use correctly.
- Rust handles this problem differently, the memory is automatically returned and cleaned once the item that owns it falls out of scope.
- The function that is called to free this memory is `drop`, it is automatically called at the closing curly bracket.

##### Variables and Data Interacting with Move
- In Rust these lines point to the same data on the heap:
  `let s1 = String::from("hello");`
  `let s2 = s1;`
- Under hood `s1` holds three values: a pointer (holding an address to memory), length, and capacity.
- The length is how much memory, in bytes, the `String` is currently using.
- The capacity is the total amount of memory, in bytes, the `String` has received from the allocator.
- When we assign `s1` to `s2` we copy the values that are on the stack, which is the pointer, length and capacity, NOT the values on the heap.
- Since both are pointing to the same place in the heap when both go out of scope we get a [[double free memory error]]. 
- This is because we are trying to free the same address in memory twice, which can cause corruption.
- To prevent this in Rust when `s1` is assigned to `s2` it loses its validity. Therefore when `s1` goes out of scope nothing needs to be freed as it is invalid.
- This concept of copying only the stack information is called a [[shallow copy]] in other languages, but in Rust since the copy invalidates the original it is called a *move*.
- With this paradigm Rust never automatically creates [[deep copies]] of your data.

##### Scope and Assignment
- When a new value is assigned to an existing variable Rust calls `drop` and frees the original value's memory immediately.
##### Variables and Data Interacting with Clone
- If a deep copy is desired we use the `clone` method, `let s2 = s1.clone();`.
- This copies everything, including the memory on the heap.

##### Stack-Only Data: Copy
- Values on the stack are not beholden to the memory strictness of those on the heap, as such all copies of primitive types are deep copies.
- If any part of a type implements the `Drop` trait the Rust compiler won't let us annotate the type with the `Copy` trait.
- As a general rule scalar types implement the `Copy` trait.
#### Ownership and Functions
- Passing a variable to a function moves or copies the value, just like an assignment.

> [!example]-
>
> ```rust
> fn main() {
> 	let s = String::from("hello"); // s comes into scope
> 	takes_ownership(s); // s's value moves into the function... 
> 					// ... and so is no longer valid here
> 	let x = 5; // x comes into scope
> 	makes_copy(x); // x would move into the function
> 				// but i32 is Copy, so it's okay to still 
> 				// use x afterward
> } // Here, x goes out of scope, then s. But because s's value was moved, nothing special happens.
> fn takes_ownership(some_string: String) { // some_string comes into scope 
> 	println!("{some_string}"); 
> } // Here, some_string goes out of scope and `drop` is called. The backing memory is freed.
> fn makes_copy(some_integer: i32) { // some_integer comes into scope 
> 	println!("{some_integer}"); 
> } // Here, some_integer goes out of scope. Nothing special happens.
> ```
##### Return Values and Scope
- Returning values also passes ownership.
- Ownership of a variable follows the same pattern: 
  Assigning a value to another variable moves it.
  When a heap variable goes out of scope the value is dropped unless it has been moved.
- With this pattern then ownership with functions becomes tedious. It would mean that every input would need to be passed back if it ever needed to be used again.
- To solve this dilemma Rust uses [[references]].
### References and Borrowing
- Instead of passing in a variable into a function we pass in a *reference* to that variable, this is works like a pointer address and is guaranteed to point to a valid value.
- References are noted with a `&` followed by the type: `&String`.
- This gives access to a value without taking ownership of it.
- To *dereference* something we use `*`.
- If a reference is used and then dropped the referenced value is still value as the dropped variable never has ownership of it.
- If a function has a reference as a parameter then it doesn't need to pass back its input since it never owned those values in the first place.
- The act of creating a reference is called [[borrowing]].
- References, like variables, are immutable by default.
#### Mutable References
- To make a reference mutable we must first make the reference mutable using `mut`.
- From there the reference can be made mutable by putting `&` and `mut` together: `&mut`.

> [!example]-
>
> ```rust
> let mut s = String::from("hello");
> let r = &mut s;
> ```

- A value can only ever ONE single mutable reference to itself.
- This restriction helps prevent [[data races]] at compile time.
- You also cannot have both immutable and mutable references to one value, it is one or the other.
- The scope of a reference is from when it is declared to its last use.
- So if a mutable reference to a value is made, used, then falls out of scope a new immutable reference can be made on that same value. They just can't both be in scope at once.
#### Dangling References
- [[Dangling pointers]] are pointer to a location in memory that has been given to someone else, i.e. that memory has bee freed but the pointer remains.
- The Rust compiler can guarantee that references till never be dangling, this is because of a Rust feature called [[lifetimes]].
#### The Rules of Reference
- At any time you can have either one mutable reference or any number of immutable ones.
- References must always be to a valid variable.
### The Slice Type
- A *slice* is a reference to contiguous sequence of elements in a collection, rather than its entirety.
- A slice does not have ownership.
#### String Slices
- A *string slice* is a reference to part of a `String` type: `&s[starting_index..ending_index]`.
- The `ending_index` is not included.
- With Rust's range syntax, `..`, if you want to start at 0 you can drop the value: `&s[..2]` is valid.
- The same is true for the last index: `&s[2..]`.
- Dropping both takes a slice of the entire string: `&s[..]`.
- `String` slices gets their own type definition: `&str`.

##### String Literals as Slices
- String literals are the same as string slices, `&str`.
- A literal is a slice pointing to the binary location.
- This is also why literals are immutable, because `&str` is an immutable reference.

##### String Slices as Parameters
- Using string slices gives greater generalisation as it can take a `String` or a reference to a `String`.
- This takes advantage of [[deref coercions]].
#### Other Slices
- Slices aren't unique to strings.
- Same as wanting to refer to a part of a `String` we may want to refer to a part of an array.
- This is done by adding the `&` before the array brackets: `&[i32]`.
## Citation
---
```
S. Klabnik and C. Nichols, "Understanding Ownership" in *The Rust Programming Language*, 2nd. J. Franklin, J. Kepler, K. Horlbeck Olsen, L. Chadwick, Eds. San Francisco, CA, USA: No Starch Press, 2023, pp. 113-137.
```