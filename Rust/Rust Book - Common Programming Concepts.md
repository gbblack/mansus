---
created at: 2024-05-22
tags:
  - basics
---
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
