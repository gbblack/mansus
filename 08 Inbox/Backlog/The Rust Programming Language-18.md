---
tags:
  - type/chapter
created:
---
[[The Rust Programming Language]]
# Patterns and Matching

> [!abstract] Summary
## Note
---
##### All the Places Patterns Can Be Used
`match` Arms
Conditional `if let` Expressions
`while let` Conditional Loops
`for` Loops
`let` Statements
Function Parameters
### Refutability: Whether a Pattern Might Fail to Match
### Pattern Syntax
Matching Literals
Matching Named Variables
Multiple Patterns
Matching Ranges of Values with `..=`
Destructuring to Break Apart Values
Ignoring Values in a Pattern
Extra Conditionals with Match Guards
@ Bindings
## Citation
---
```
S. Klabnik and C. Nichols, "Common Programming Concepts" in *The Rust Programming Language*, 2nd. J. Franklin, J. Kepler, K. Horlbeck Olsen, L. Chadwick, Eds. San Francisco, CA, USA: No Starch Press, 2023, pp. 48-?.
```
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