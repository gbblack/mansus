_Patterns_ are a special syntax for matching against the structure of types. Using patterns along with the `match` [[Rust Book - Match|expression]] and other constructs can give greater control over a programs flow.

A pattern consists of some combination of:
- Literals
- Destructured array, enums, structs, or tuples
- Variables
- Wildcards
- Placeholders

To use a pattern we compare it to some value, if its a match then we use the value in our code. 

`match` Arms

Patterns are most common in the arms of a  `match` expression.

```txt
match VALUE {
    PATTERN => EXPRESSION,
    PATTERN => EXPRESSION,
    PATTERN => EXPRESSION,
}
```

A requirement for a `match` expression is that they be exhaustive in that all possibilities for the value must be accounted for. One way to do this is to have a catchall pattern for the final arm.

The pattern for a catchall is `_` and will match to anything but never bind to a variable.

##### Conditional `if let` Expressions

We can use a mixture of `if let`, `else if` and `else if let` expressions to create a more flexible `match` expression as each arm doesn't need to relate to each other unlike `match`.

In the example below the function determines what color the background should be based on a number of criteria and conditions. \

```rust
fn main() {
    let favorite_color: Option<&str> = None;
    let is_tuesday = false;
    let age: Result<u8, _> = "34".parse();

    if let Some(color) = favorite_color {
        println!("Using your favorite color, {color}, as the background");
    } else if is_tuesday {
        println!("Tuesday is green day!");
    } else if let Ok(age) = age {
        if age > 30 {
            println!("Using purple as the background color");
        } else {
            println!("Using orange as the background color");
        }
    } else {
        println!("Using blue as the background color");
    }
}
```

This structure allows us to support more complex pattern requirements. However using these expressions means the compiler does not check for exhaustiveness.

##### `while let` Conditional Loops

Similar to the above conditionals a `while let` loop allows for a loop to continue so long as it keeps matching a pattern.

```rust
let mut stack = Vec::new();

stack.push(1);
stack.push(2);
stack.push(3);

while let Some(top) = stack.pop() {
	println!("{top}");
}
```

in this example while top has a value the loop continues.

##### `for` Loops

In a `for` loop the value immediately after the keyword is a pattern. In `for x in y`  the `x` is the pattern.

##### `let` Statements

The `let` statement itself is a pattern.

`let PATTERN = EXPRESSION;`

More complicated example, matching a tuple against a pattern:

`let (x, y, z) = (1, 2, 3);`

##### Function Parameters
Function Parameters can also be patterns and function similarly to the `let` patterns. They can match with tuples like so: `fn print_coordinates(&(x, y): &(i32, i32)) {}`.

##### Refutability: Whether a Pattern Might Fail to Match

Patterns can be either _refutable_ or _irrefutable_. Patterns that will match with any possible value are _irrefutable_, such as `let x = 5;` where the pattern x will match with any value. Patterns that can fail to match are _refutable_ such as `Some(x)` because it cannot be assigned the value `None`.

Function parameters, `let` statements and `for` loops may only accept irrefutable patterns were as the conditional `if let` and `while let` can accept both.