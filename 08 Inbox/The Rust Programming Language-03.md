---
tags:
  - type/chapter
created: 2024-12-12
---
[[The Rust Programming Language]]
# **Common Programming Concepts**
### **Note**
---
##### **Variables and Mutability**
- Variables are [[immutable]] by default, declared by `let`.
- This means once a variable is assigned a value it cannot be changed.
- A variable can be made mutable by adding the `mut` keyword: `let mut <var>`.

**Constants**
- Constants are similar to immutable variables in that once a value is assigned it cannot be changed.
- They differ in that you can't use `mut` on a constant, declared by `const`.
- A constant *must* have an annotated type.
- Constants can be declared in any [[scope]], including global.
- Finally constants must be assigned a constant expression, a value that is computed at runtime cannot be a constant.
- The naming convention for constant is ALL_CAPS.
- Constants are valid for as long as the program is running and within the scope it was declared in.
- A good use for constants is using them for hard coded values.

**Shadowing**
- A variable can be redeclared using the same name as a previous one.
- This is called [[shadowing]], with the first variable being shadowed by the second.
- The variable can regress to the original value once the shadow (second) variable falls out of scope. 
- Shadowing is different than the `mut` keyword because it essentially creates a whole new variable, we may even change the type in doing so.
##### **Data Types**
- All values in Rust have a Data Type, telling the compiler what kind of data the variable holds.
- This makes Rust a [[statically-typed]] language.
- To bind a type to a variable we annotate it, `let guess: u32`.
- The annotation is the value happening after `:`.

**Scalar Types**
- A [[scalar]] is a single value type. There are four categories of scalar: integers, floats, boolean and characters.

Integer Types
- An [[integer]] is a number without a fraction, a whole number.
- Integers can be signed (`i`) or unsigned (`u`).
- Unsigned integers can never be negative while signed can be either. 
- The sign value is followed by the bit count, showing the allocated space for that number value.

> [!example]- Integer Types
> |Length|Signed|Unsigned|
|---|---|---|
|8-bit|`i8`|`u8`|
|16-bit|`i16`|`u16`|
|32-bit|`i32`|`u32`|
|64-bit|`i64`|`u64`|
|128-bit|`i128`|`u128`|
|arch|`isize`|`usize`|
> 

- Signed numbers are stored using [[two's complement]] representation.
- Each signed number can hold these values (where n is the bit size): `-(2n-1) to 2n-1-1`.
- Each unsigned number can hold these values: `0 to 2n-1`.
- The `isize` and `usize` integers are related to the architecture of the computer your code is running on, used mostly for [[indexing]] a collection.
- Integer literals can be written in several number systems, such as: [[binary]], [[hexadecimal]] or [[octal]].
- When writing numbers we can include `_` as a visual separator.

> [!example]- Number Literals
> |Number literals|Example|
|---|---|
|Decimal|`98_222`|
|Hex|`0xff`|
|Octal|`0o77`|
|Binary|`0b1111_0000`|
|Byte (`u8` only)|`b'A'`|

- The Rust default for integer is `i32`.
- When trying to store a value outside the allocated size of the integer an [[Integer Overflow]] error will occur which will cause your program to [[panic]]. 
- When using the `--release` tag to build your code the compiler does NOT check for integer overflow. If it does occur then Rust uses [[two's complement wrapping]] to handle it.

Floating-Point Types
- Floating-Point numbers are numerical values that have decimal points.
- The two types are: `f32` and `f64` which are 32 and 64 bit in size.
- The default is `f64`.
- All floating points are signed.
- `f32` is a [[single-precision]] float.
- `f64` is a [[double-precision]] float.

Numeric Operations
- Rust supports all the basic mathematical operations: addition, subtraction, multiplication, division and remainder.
- Integer division truncates to 0 of the nearest number, which means that it divides closer to 0 regardless of sign. (i.e. `-5/2= -2.5 --truncates to-> -2).

> [!example]- Operators
>|Operator|Example|Explanation|
|---|---|---|
| `+`  | `expr + expr`| Arithmetic addition|
| `+=` | `var += expr`| Arithmetic addition and assignment|
|---|---|---|
| `-`  | `- expr`| Arithmetic negation|
| `-`  | `expr - expr`| Arithmetic subtraction|
| `-=` | `var -= expr`| Arithmetic subtraction and assignment
|---|---|---|
| `*`  | `expr * expr`| Arithmetic multiplication|
| `*=` | `var *= expr`| Arithmetic multiplication and assignment|
|---|---|---|
| `/`  | `expr / expr`| Arithmetic division|
| `/=` | `var /= expr`| Arithmetic division and assignment
|---|---|---|
|`%`|`expr % expr`| Arithmetic remainder|
|`%=`|`var %= expr`| Arithmetic remainder and assignment|

The Boolean Type
- Rust [[boolean]] type is either `true` or `false` and is annotated with `bool`.

The Character Type
- The [[char]] type is Rust's most basic alphabetical type.
- char literals must be specified with single quotes, `let c: char = 'c';`.
- This type is 4 bytes and represents the [[Unicode Scalar Value]], not [[ASCII]].

**Compound Types**
- Compound types are those that combine multiple values into one type.

The Tuple Type
- A [[tuple]] is a general way to gather several values into one structure.
- They can contain elements of different types but once declared they are of a fixed size that cannot be changed.
- A tuple in Rust is written inside parentheses with comma separated values: 
  `let tup: (i32, f64, u8) = (500, 6.4, 1);`.
- The variable, `tup`, binds to the entire tuple and is now considered a single compound element.
- To access the individual elements we can use [[pattern matching]] to [[destructure]] it:
  `let (x, y, z) = tup;`.
- [[Dot notation]] can also be used to access the elements:
  `let five_hundred = x.0;`.
- A tuple without any elements in Rust has a special name, *unit*. A [[unit tuple]]'s value and type are both `()`.
- They represent an empty value or empty return type, all [[expressions]] implicitly return a unit tuple if they return nothing else.

The Array Type
- [[Arrays]] must have elements of the same type and have a fixed length upon declaration.
- Arrays are written with `[]`, `let arr = [1,2,3,4];`.
- Arrays are best used when you want data stored on the [[stack]] or when you want a guaranteed fixed size of elements.
- Arrays are less flexible than [[vectors]], which is allowed to shrink or grow.
- When typing an array within square brackets we have the element type and the size of the array: `let arr: [i32; 5] = [1,2,3,4,5];`.
- To initialise an array where every element has the same value we can do:
  `let arr: [i32; 100] = [0; 100]; // an array of 100 os`.
- To access an element in a array we can use its index: `let value: i32 = arr[0];`.
##### **Functions**
- A common entry point to programs is the function `main`.
- They keyword for declaring a function is `fn`.
- The Rust naming convention for function names uses `snake_case`.
- To define a function we use the keyword `fn`, followed by: the name, the parameters in parentheses and then curly brackets for the body. (`fn main() {};`).

> [!example]- Example Function
> ```rust
> fn main() {
> 	println!("Hello, world!");
> 	another_function();	
> };
> 
> fn another_function() {
> 	println!("Another function.");
> }
> ```

- We can call a function that has been defined using the function name + parentheses containing any of its parameters.
- Rust doesn't care about the order in which we declare our functions so long as the caller has access to the scope the function is defined in.

**Parameters**
In a function signature a parameter must ben typed.

A function body is split between statements and expressions:
- **Statements**: instructions that perform some action but do not return a value
- **Expressions**: evaluate to a resultant value

`let y = 6` is a statement as an action if storing the value 6 into y id an action.
The `6` in this statement is an expression as it is evaluating to 6.

Expressions do not include ending semicolons.

##### Return Values

Functions can have return values. We must type the return value using an `->` in the signature.

The return value is the same as the final expression in rust, if you need to return early then use the `return` keyword.

```rust
// will return 5
fn five() -> i32 { 5 }
```

Since 5 is an expression we do not put a semicolon after it else we get an error.

This is because putting a `;` turns the line into a statement which doesn't evaluate to a value, meanwhile the compiler is expecting a returned value os `i32`.

When referring to strings in rust it is either `String` or `&str`.

Many operation used on vectors can be used on `String` because it's really a wrapper around a vector of bytes.

```rust
let mut s = String::new();
```

This creates a new empty string. To make a string literal (`"something"`) a `String` we can call the `.to_string()` method on the literal. The function `String::from(literal)` also works.

**Statements and Expressions**
**Functions with Return Values**
##### **Comments**
##### **Control Flow**
**`if` Expressions**
Handling Multiple Conditions with `else if`
Using `if` in a `let` Statement
**Repetition with Loops**
Repeating Code with `loop`
Returning Values from Loops
Loop Labels to Disambiguate Between Multiple Loops
Conditional Loops with `while`
Looping Through a Collection with `for`
##### **Citation**
---
```
S. Klabnik and C. Nichols, "Common Programming Concepts" in *The Rust Programming Language*, 2nd. J. Franklin, J. Kepler, K. Horlbeck Olsen, L. Chadwick, Eds. San Francisco, CA, USA: No Starch Press, 2023, pp. 74-112.
```
---
##### Completion Checklist
**For a technical text:**
###### II. From Dark to Dawn
- [ ] Wait at least 5mins before beginning this section.
- [ ] In the `Note` section breakdown the chapter into its subheading: all the sections in bold.
- [ ] In bullet points, under each section and sub section heading summarise the major points of that section. In these bullet point make links to anything that could be referenced in a permanent note. This will take awhile.
- [ ] While summating the sections copy paste interesting sections into the note under `Highlights`. Include the page number right above the block. These highlights should only be the author's own reflections that you think are interesting, nothing definitive. There may be nothing.
- [ ] Update status tag to `status/dawn`.
###### III. From Dawn to Day
- [ ] Write the chapter `Summary`.
- [ ] Remove or complete the `Nota Bene` as necessary.
- [ ] Fill in the context tags for the metadata.
- [ ] Update status tag to `status/day`.
- [ ] Remove this checklist.

***


### if else statement

In Rust `if` statements are written as:

```rust
if condition {
	...
} else if {
	...
} else {
	...
}
```

The condition must be a boolean.

Since if is an expression it can be used on the right hand side (RHS) of the let assigning keyword.

```rust
let condition = true; 
let number = if condition { 5 } else { 6 }; 
println!("The value of number is: {number}");
```

### Loops

Rust has three kinds of loops: `loop`, `while`, `for`.

##### Loop

The `loop` keyword tells rust to infinitely loop this block of code until you tell it to stop.
To stop the loop use the `break` keyword.
The `continue` keyword is used in any loop to tell program to skip the remaining code and start another iteration.

To return a value from within a loop you can add an expression to the same line that the `break` keyword is used in.

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {result}");
}
```

In this example the `;` used in the break line is used to end the statement assigning the new counter value and the counter variable is returned.

You can give loop labels to help distinguish them from on another.

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```

the `break` and `continue` keywords apply to the innermost loop but with labelling you cna specify exactly which loop they should apply to.

##### While

The `while` loop is a loop that iterates so long as a condition is true. When the condition becomes false or a `break` statement is used the loop ends.

```rust
while condition {
	...
}
```

##### For

The `for` loop is designed to iterate over a collection of elements.

```rust
for element in arr {
	...
}
```

You cannot annotate the type for the variable inside the `for` loop statement. (i.e. `example`) but what you can do is use an [[Rust - integer suffix]] on one of the literals in the range, type inference in the compiler can use that to logic the rest. 

You can also cast the whole statement using `as`.

```rust
for i in 1..100 as i64 {
    // ...
}
```

