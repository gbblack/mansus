---
tags:
  - type/chapter
created:
---
[[The Rust Programming Language]]
# Common Collections

> [!abstract] Summary
## Note
---
##### Storing Lists of Values with Vectors
Creating a New Vector
Updating a Vector
Reading Elements of Vectors
Iterating over the Values in a Vector
Using an Enum to Stor Multiple Types
Dropping a Vector Drops Its Elements
##### Storing UTF-8 Encoded Text With Strings
What Is a String?
Creating a New String
Updating a String
Indexing Into Strings
Slicing Strings
Methods for Iterating Over Strings
Strings Are Not So Simple
##### Storing Keys with Associated Values in Hash Maps
Creating a New Hash Map
Accessing Values in a Hash Map
Hash Maps and Ownership
Updating a Hash Map
Hashing Functions
## Citation
---
```
S. Klabnik and C. Nichols, "Common Programming Concepts" in *The Rust Programming Language*, 2nd. J. Franklin, J. Kepler, K. Horlbeck Olsen, L. Chadwick, Eds. San Francisco, CA, USA: No Starch Press, 2023, pp. 48-?.
```

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

common data structures are called collections in Rust. Collections contain data of multiple types under one grouping. These collections are always stored on the heap.

- **Vector**: allows for a variable number of values of the same type to be stored together.
- **String**: a collection of characters.
- **Hash Map**: like a dictionary (py) or object (js), also known as a map.

other types of  [collections](https://doc.rust-lang.org/std/collections/index.html)

##### Storing with Vectors

To create a new empty vector we call `Vec::new` like this:

```rust
let v: Vec<i32> = Vec::new();
```

Vectors are implemented using [[Rust Book - Generics|generics]], `<>`, and if the vector is created empty we must annotate the type so the compiler knows what kind of values to expect.

If we are creating the vector with some values already included we can use this [[macros]]: `vec!`.
This creates a vector already initialised with the values you pass it.

```rust
let v = vec![1, 2, 3];
```

To add to a vector we use the `push` method. We must make the vector mutable to do this.

```rust
let mut v = Vec::new();

v.push(5);
v.push(6);
v.push(7);
v.push(8);
```

##### Reading a Vector

You can access a value in a vector through indexing or the get method. 

To access via index, call: `&<name>[index]`, return type: `&i32`
To access via get, call: `<name>.get(index)`, return type: `Option<&i32>`

The reason for the difference is so we can choose how our program behaves when we try to access a non-existing value. 

Via index would cause rust to panic is there is no element at index 2, causing the program to crash.

Via get the function would return `None`, which is the other [[Rust Book - Enums#The `Option` Enum And Its Advantages Over Null Values|Option]] variant is something is not `Some`. This allows the program to continue if we don't want it to crash at this point.

##### Iterating over a Vector

To iterate over a vector we can use a for loop that looks a lot like pythons:

```rust
let v = vec![100, 32, 57];
for i in &v {
	println!("{i}");
}
```

If we wan to make changes to the values we need to make the vector and the loop mutable.

```rust
let mut v = vec![100, 32, 57];
for i in &mut v {
	*i += 50;
}
```

Remember that `*` is for dereference which is required to change the values of the original vector. More here: [[Dereference Operator]].

##### Using an Enum to Store Multiple Types

Vectors can only store values of the same type, this is inconvenient. A smart workaround would be to use enums. The type the vector would hold would be the enum, but each of its variants can be of a different type.

```rust
enum SpreadsheetCell {
	Int(i32),
	Float(f64),
	Text(String),
}

let row = vec![
	SpreadsheetCell::Int(3),
	SpreadsheetCell::Text(String::from("blue")),
	SpreadsheetCell::Float(10.12),
];
```

##### Dropping a Vector

When a vector goes out of scope, so too does all of its values.

```rust
{
	let v = vec![1, 2, 3, 4];

	// do stuff with v
} // <- v goes out of scope and is freed here
```

More info: [vector docs](https://doc.rust-lang.org/std/vec/struct.Vec.html)

##### Storing UTF-8 Encoded Text with Strings

[[Rust Book - String|Strings]] are technically a collection of bytes so they fall under the same category of contruct as the above Vectors. 

When referring to a *string* in the core language we mean the string slice `str` and its borrowed counterpart `&str`. 

The `String` type however is on provided byt the standard library and not part of the core language.

##### Creating a new String

Many operations possible on `Vec<T>` are available for `String` because as mentioned before it is actually a wrapper for a collection of bytes. 

```rust
let mut s = String::new();
```

This creates a new string called `s` which is mutable so we can load data into it. When trying to take a string literal and turn it into a String collection we use the `to_string` method.

```rust
let data = "initial string";
let s = data.to_string();

// or directly
let s = "inintial string".to_string()
```

To do this we can also use the function `String::from`.

```rust
let s = String::from("initial string");
```

##### Updating a String

When growing or updating a string you can treat it just like a Vector by pushing or popping elements in and out of it. You can also use `+` operator or `format!` [[macros]] to concatenate a string.

To append to a string we use the `push_str` method.

```rust
let mut s = String::from("foo");
s.push_str("bar"); // foobar
```

When concatenating with `+` rust can coerces disparate string types together, i.e. `str` and `String`, [[Rust Book - Smart Pointers|see more]].

when needing to concatenate mutliple string its best to use the `format!` macro.

```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{s1}-{s2}-{s3}");
```

##### Indexing into Strings

In rust you cannot access a character through indexing. Instead you have to use a slice (range) every time. if you're string is `let word = "hello"` and you want the second letter you'd have to get it like so: `let s = &word[1..2]`.

The best way to navigate individual parts of a string is through for loops as they require you to be specific about what kind of string data you want. [see more](https://doc.rust-lang.org/book/ch08-02-strings.html)

### Storing Keys with Associated Values in Hash Maps

The last collection is the *hash map*, defined as `HashMap<K, V>`. This stores a mapping of keys of type `K` to values of type `V` using a *hash function*. These are named many different ways across programming languages: dictionary, object, map hash table. 

### Creating a new hash map

To create a hash map you use the keyword `new` and to add elements we use `insert`.

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

Hash maps, like [[Rust Book - Common Collections#Storing with Vectors|vectors]] store their data on the [[heap]].

Also like vectors all the keys must be of the same type and all the values must be of the same type.

##### Accessing Values in a Hash Map

To retrieve values from a map we use `get`.

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

let team_name = String::from("Blue");
let score = scores.get(&team_name).copied().unwrap_or(0);
```

##### Hash Maps and Ownership

hash maps can contain different levels of [[Rust Book - Ownership|ownership]]. If stored values contain the [[Copy Trait]] then the value keeps its ownership, if however it does not have this trait then ownership is passed onto the hash map. [[Rust Book - Smart Pointers|see more]]

