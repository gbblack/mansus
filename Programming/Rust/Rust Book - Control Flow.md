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

