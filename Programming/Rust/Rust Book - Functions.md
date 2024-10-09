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