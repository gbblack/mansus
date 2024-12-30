---
tags:
  - type/chapter
  - status/day
created: 2024-12-12
---
[[The Rust Programming Language]]
# **Common Programming Concepts**

> [!abstract] Summary
> Basic programming concepts as they apply to Rust. 
## **Note**
---
### **Variables and Mutability**
- Variables are [[immutable]] by default, declared by `let`.
- This means once a variable is assigned a value it cannot be changed.
- A variable can be made mutable by adding the `mut` keyword: `let mut <var>`.

##### **Constants**
- Constants are similar to immutable variables in that once a value is assigned it cannot be changed.
- They differ in that you can't use `mut` on a constant, declared by `const`.
- A constant *must* have an annotated type.
- Constants can be declared in any [[scope]], including global.
- Finally constants must be assigned a constant expression, a value that is computed at runtime cannot be a constant.
- The naming convention for constant is ALL_CAPS.
- Constants are valid for as long as the program is running and within the scope it was declared in.
- A good use for constants is using them for hard coded values.

##### **Shadowing**
- A variable can be redeclared using the same name as a previous one.
- This is called [[shadowing]], with the first variable being shadowed by the second.
- The variable can regress to the original value once the shadow (second) variable falls out of scope. 
- Shadowing is different than the `mut` keyword because it essentially creates a whole new variable, we may even change the type in doing so.
### **Data Types**
- All values in Rust have a Data Type, telling the compiler what kind of data the variable holds.
- This makes Rust a [[statically-typed]] language.
- To bind a type to a variable we annotate it, `let guess: u32`.
- The annotation is the value happening after `:`.

##### **Scalar Types**
- A [[scalar]] is a single value type. There are four categories of scalar: integers, floats, boolean and characters.

**Integer Types**
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

**Floating-Point Types**
- Floating-Point numbers are numerical values that have decimal points.
- The two types are: `f32` and `f64` which are 32 and 64 bit in size.
- The default is `f64`.
- All floating points are signed.
- `f32` is a [[single-precision]] float.
- `f64` is a [[double-precision]] float.

**Numeric Operations**
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

**The Boolean Type**
- Rust [[boolean]] type is either `true` or `false` and is annotated with `bool`.

**The Character Type**
- The [[char]] type is Rust's most basic alphabetical type.
- char literals must be specified with single quotes, `let c: char = 'c';`.
- This type is 4 bytes and represents the [[Unicode Scalar Value]], not [[ASCII]].
##### **Compound Types**
- Compound types are those that combine multiple values into one type.

**The Tuple Type**
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

**The Array Type**
- [[Arrays]] must have elements of the same type and have a fixed length upon declaration.
- Arrays are written with `[]`, `let arr = [1,2,3,4];`.
- Arrays are best used when you want data stored on the [[stack]] or when you want a guaranteed fixed size of elements.
- Arrays are less flexible than [[vectors]], which is allowed to shrink or grow.
- When typing an array within square brackets we have the element type and the size of the array: `let arr: [i32; 5] = [1,2,3,4,5];`.
- To initialise an array where every element has the same value we can do:
  `let arr: [i32; 100] = [0; 100]; // an array of 100 os`.
- To access an element in a array we can use its index: `let value: i32 = arr[0];`.
### **Functions**
- A common entry point to programs is the function `main`.
- They keyword for declaring a function is `fn`.
- The Rust naming convention for function names uses `snake_case`.
- The [[function signature]] in Rust looks like the keyword `fn`, followed by: the name, the parameters in parentheses and then curly brackets for the body. (`fn main() {};`).

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
##### **Parameters**
- Parameters are special variables that are a part of a function's signature.
- The words arguments and parameters are used interchangeably to refer to these values.
- In the example below the function `another_function` has one argument, `x` that is of type `i32`.

> [!example]
> ```rust
> fn another_function(x:i32) {
> 	println!("The value of x is: {x}");
> };
> ```

- In a function signature all parameters *must* be typed.
##### **Statements and Expressions**
- A function body is made out of a series of statements, ended optionally by an expression.
- Rust is unique in that there is a distinction between statements and expressions.
- A [[statement]] is: instructions that perform some action but do not return a value, `let y=6`.
- An [[expression]]: evaluates to a resultant value, `y + 1`.
- Examples of expression: calling a function, calling a macro, a new scope with curly brackets.
- Expressions do not include `;`, doing so turns the expression into a statement.
- This is because ending the expression with `;` means there is no return value and expressions must return something.
##### **Functions with Return Values**
- Functions can return values to the code that calls them, we so not name the return but type is indicated with `->`, at the end of the signature.
- In Rust the return value is the same as the final expression in the function body.
- As such most return do NOT use the `return` keyword and are instead passed implicitly.
- The `return` keyword is for returning early from a function block.

> [!example] Implicit Final Expression Return
>```rust
fn five() -> i32 { 5 } // returns 5

- Number literals are expression on their own and return their own value.
- Since functions can return a value we can bind the returned value to a variable in a statement, `let x = five()`;
### **Comments**
- A single line comment in Rust uses: `//`.
- Rust also supports [[documentation comments]] as seen [[The Rust Programming Language-14|here]].
### **Control Flow**
- Control Flow constructs allow us to control the execution of the code base on certain conditions.
##### **`if` Expressions**
- An `if` expression allows for the branching of execution based on whether or not a provided condition is met.
- In Rust the `if` expression is written: `if condition {}`.
- Unlike some other languages the condition is NOT held inside parentheses.
- Blocks of code associated with the `if` condition are called *arms*, same as the arms of the match condition.
- The `else` condition is an optional expression if the condition fails, if not provided the program will skip over the `if` arm.
- An `if else` block is written as: `if condition {} else {}`.
- The condition *must* evaluate to a boolean.
##### **Handling Multiple Conditions with `else if`**
- Multiple conditions can be chained together use an `else if` block.
- Written as: `if condition {} else if condition {} ... else {}`.
- Using this block can cause messy code quickly, a better branching construct for this situation would be [[match]].
##### **Using `if` in a `let` Statement**
- Since `if` is an expression we can use it to assign a value with the `let` statement, `let number: u32 = if condition {} else {};`.
- In this case both blocks must return a value of the same type.
##### **Repetition with Loops**
- For iterating over blocks of code multiple times programming languages use loops.
- Rust has three kinds of loops: `loop`. `while` and `for`.
##### **Repeating Code with `loop`**
- The `loop` keyword tells Rust to iterate over this block of code forever until it is explicitly told to stop.
- An example: `loop { println!("again"); }`.
- To break this loop we add the keyword `break` within the loop block.
- The keyword `continue` can be used to tell the loop to skip over remaining code and return to the start.
##### **Returning Values from Loops**
- To return a value from a loop the expression must be after the `break` expression.
- You can also use `return` from inside the loop to leave it as that always exits the current function.
##### **Loop Labels to Disambiguate Between Multiple Loops**
- If you have nested loops we can use labels to differentiate them,
  `'big_loop loop { 'little_loop loop {} }`.
##### **Conditional Loops with `while`**
- A conditional loop is one that only runs if a condition is true.
- A simple way to do this is using `while`, `while condition {}`.
- So long as the condition is met the `while` loop will run.
##### **Looping Through a Collection with `for`**
- To loop over elements in a collection the best method is `for`.
- Written as: `for element in collection {}`.
- This prevents out of bounds errors as it iterates over every element in a collection an no more.
##### **Citation**
---
```
S. Klabnik and C. Nichols, "Common Programming Concepts" in *The Rust Programming Language*, 2nd. J. Franklin, J. Kepler, K. Horlbeck Olsen, L. Chadwick, Eds. San Francisco, CA, USA: No Starch Press, 2023, pp. 74-112.
```
