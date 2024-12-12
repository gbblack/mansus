---
tags:
  - type/book_chapter
created: 2024-12-12
---
[[The Rust Programming Language]]
# **Common Programming Concepts**

> [!abstract] Summary
### **Note**
---
##### **Variables and Mutability**
**Constants**
**Shadowing**
##### **Data Types**
**Scalar Types**
Integer Types
Floating-Point Types
Numeric Operations
The Boolean Type
The Character Type
**Compound Types**
The Tuple Type
The Array Type
##### **Functions**
**Parameters**
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

***
### Variables and Mutability

**Variables**

Variables by default are immutable.
Variables are declared with `let`.
If you want to make a variable mutable use `let mut`.

`let` variables cannot be used in global scope.

**Constants**

Constants are like variables and are immutable by default, but unlike variables they cannot use the `mut` keyword and thus never be mutated.

Constants are declared with `const`. The type of the variable **must** be declared.
Constants can be declared in any scope and are very useful for holding information that needs to be accessed throughout the program.

Rust's name in convention for constants `IS_LIKE_THIS`.

They are often used for hardcoded values that need to be referenced by other parts of the code, such as the max of a range or the speed of light.

**Shadowing**

Shadowing is when you declare a new variable with `let` using the same name as an existing old one, i.e.:

```rust
let x = 12;
let x = 10;
```

This code compiles, and the first variable is *shadowed* by the second.

This allows for transformations to occur on immutable data:

```rust
let x = 12;
let x = x - 2;
```

This code compiles and i asked to return x will give us 10.
Shadowing ends within scope so for example:

```rust
let x = 12;
let x = x - 2;
{
	let x = x + 10;
	println!("x is {x}")
}
println!("x is {x}")
```

The first `println!` will give 20, the second will give 10. This is because the latest shadow was inside a different code block and once that block ended so did the scope of that shadow, returning to a previous version.

You can have as many shadows as needed.
Shadows, unlike `mut`, can change types. So this compiles:

```rust
let x = 12;
let x = "hello";
```





---


Two types: scalar compound

Rust is statically types so the compiler needs to know the type of all variables to succeed.

The compiler can make best guess to compile based on the value being stored like seen when shadowing. But it's better to annotate the variable with the type for clarity.

```rust
let num: u32 = 22
```

### Scalar Types

##### Integers

| Length    | Signed  | Unsigned |
| --------- | ------- | -------- |
| `8-bit`   | `i8`    | `u8`     |
| `16-bit`  | `i16`   | `u16`    |
| `32-bit`  | `i32`   | `u32`    |
| `64-bit`  | `i64`   | `u64`    |
| `128-bit` | `i128`  | `u128`   |
| `arch`    | `isize` | `usize`  |

The difference betwen signed and unsigned is whether or not the umber can be negative.
i.e. if i has a sign (-) or not. So a number that can only be positive is unsigned, xero is a positive and even number.

`isize` and `usize` depend on the architecture of the computer the program is runnign on, arch is for architecture.

You can write integers with an `_` in the number to make it easier to read and it can be in any of these forms:

| Number Literals | Example       |
| --------------- | ------------- |
| Decimal         | `98_222`      |
| Hex             | `0xff`        |
| Octal           | `0o77`        |
| Binary          | `0b1111_0000` |
| Byte(`u8` only) | `b'A'`        |

When in doubt on which integer to use, simply use the default: `i32` or `u32`

The primary use case for `isize` and `usize` is for indexing a collection.

If you try to store a number bigger than the type capacity you'll receive an integer overflow error.

##### Floating Point Types

`f32` and `f64` are the floating point types. `f64` is the default, all floating types are signed.

##### Numeric Operations

Rust support these 5 basic numeric operations:

Addition: `sum = 5 + 10`
Subtraction: `difference = 54.5 - 3.2`
Multiplication: `prodct = 4 * 30`
Division: 
- `quotient = 50 / 25`
- `truncated = -5 / 3 // returns -1`
Remainder: `remainder = 45 % 9`

##### Boolean Type

Booleans are on byte in size and are either `true` or `false`

##### Character Type

The `char` type is the most primitive alphabetic type. It is four bytes and represents Unicode Scalar Value. It also is marked with single quotes not double.

Written as:

```rust
let c = 'z';
let z: char = 'Z';
let emoji = '😻'
```

### Compound Types

This is how Rust can group multiple values of different types into one.

##### Tuple Type

A tuple is a general way to group values of multiple types into one. They have a fixed length on declaration that cannot be changed.

A tuple is created by writing a list of values inside `()` separated by commas.
It also can be declared explicitly with the type `tup` along with a mirror of the tuple that holds the expected types for each value.

```rust
let tup: (i32, f64, u8) - (500, 6.4, 1);
```

Values can be reached through pattern matching like this:

```rust
let tup = (500, 6.4, 1);
// grabbing values
let (x, y, z) = tup

println!("The value of y is {y}");
```

By creating some variables in the same pattern as the tuple each element is then assigned to its matching variable. This is called destructuring because we're breaking down a greater structure. We can also use dot notation (with the index of value) to access a tuple:
```rust
let five_hundred = tup.0 // returns 500
```

If a tuple has no values it gets a special name `unit` and it's type is `()` marking an empty tuple. An expression implicitly returns a unit value if they don't have any other value to return.

##### Array Type

Arrays must have elements of the same type and have a fixed length upon declaration.
We write arrays using `[]`.

```rust
let arr = [1,2,3,4];
```

Arrays are most useful when you want data on the stack and not the heap, or when you want to guarantee a fixed number of elements.
Arrays are best used when paired with known data such as the 12 months in a year or the size of an inventory slot.

This is how you write a strongly typed array, first with the type of the elements and then how many: 

```rust
let arr: [i32; 5] = [1,2,3,4,5];
let arr: [i32; 100] = [0; 100] // initialises an array with 100 elements of 0
```

To access elements in the array you use the index:
```rust
let arr: [i32; 5] = [1,2,3,4,5];

let first_value: i32 = arr[0];
```

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

The keyword for functions is `fn`.
Rust convention is to use `snake_case` for naming.

The structure of a function is this: 

```rust
fn function_name(params) {
	...
}
```

You can define your functions anywhere in a file rust doesn't care only that if on call another the one being called is within scope.

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

