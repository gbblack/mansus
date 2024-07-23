
Two types: scalar compound

Rust is statically types so the compilerr needs to know the type of all variables to suceed.

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
```

To access elements in the array you use the index:
```rust
let arr: [i32; 5] = [1,2,3,4,5];

let first_value: i32 = arr[0];
```

