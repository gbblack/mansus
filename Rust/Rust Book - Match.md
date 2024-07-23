Rust has a flow construct called `match` similar to the switch construct found in Javascript.

This construct compares values against a series of patterns and execute the code that has the matching pattern.

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

So in this example if Coin matches the Penny type return 1, 5 for Nickel and so on.

N.B. if you want to write multiples line in each arm of the match you must use `{}`.

##### Patterns That Bind To Values

We can also use match patterns with variables using typed enums.

```rust
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {state:?}!");
            25
        }
    }
}
```

##### Matches are Exhaustive

The arms if a `match` block must cover all possible states otherwise it will not compile.

Since all states must be exhaustive we can use the `other` state as a sort of default state wherein the variable matched none of the arms therefore the other arm is run.

Like in this example where if a player rolls a 3 or a 7 two different arms get executed, but in all other cases of the die roll the `other` arm is executed.

```rust
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	other => move_player(other),
}

fn add_fancy_hat() {}
fn remove_fancy_hat() {}
fn move_player(num_spaces: u8) {}
```

In the case we want a catch all value but don't want to use it we use `_` to mark the arm.

```rust
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	_ => reroll(),
}

fn add_fancy_hat() {}
fn remove_fancy_hat() {}
fn reroll() {}
```

What this does is keep the the match block going until it hits an arm that we have defined.

If we want an arm to do nothing when its executed we can use the [[Rust Book - Data Types#Tuple Type|unit tuple]] to do nothing.

```rust
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	_ => (),
}

fn add_fancy_hat() {}
fn remove_fancy_hat() {}
```

More in [[Chapter 18]]

##### `if let` Control Flow

The `if let` syntax combines the keywords `if` and `let` into one more concise expression to match one pattern and ignore the rest. Using `if let` we can simplify this code block.

```rust
let config_max = Some(3u8);
match config_max {
	Some(max) => println!("The maximum is configured to be {max}"),
	_ => (),
}
```

Into this one

```rust
let config_max = Some(3u8);
if let Some(max) = config_max { 
	println!("The maximum is configured to be {max}");
}
```

In the original block we check to see if the variable matches the Some condition but to be exhaustive we must include the `_` catch all state, otherwise rust wouldn't compile.

The `if let` syntax covers for that catch all condition, it says if condition is true let value be assigned, therefore no match statement is needed. 