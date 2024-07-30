A significant influence on Rust is [[functional programming]]. To program in a functional style we use functions as values passed into other functions, return function, assign functions to variables and more.

Rust share some of this functionality through these features:

- Closures: a function like construct to store a variable
- Iterators: a way of processing a series of elements

### Closures

Rust's closures are [[anonymous functions]] that can be saved into a variable or passed as an argument into another function. 

Here is an example of a closure that captures the `x` variable:

```rust
|val| val + x
```

It works similarly to arrow syntax in Typescript.

```ts
(val) => { val + x };
```

Critically both of these statement should be stored inside a variable to be of use.

Characteristics of a closure include:
- using `||` instead of `{}` around input variables
- optional body delimitation (`{}`) for a single line expression (usually mandatory)
- the ability to capture the outer environment variables

The power of closures is that you can create it in one place and then call and evaluate it elsewhere in a different context.

[more info](https://doc.rust-lang.org/rust-by-example/fn/closures.html)

##### Capturing the Environment with Closures

Closures can capture variables from within the environment they're created in, unlike functions. Imagine this scenario:

>Every so often, our t-shirt company gives away an exclusive, limited-edition shirt to someone on our mailing list as a promotion. People on the mailing list can optionally add their favorite color to their profile. If the person chosen for a free shirt has their favorite color set, they get that color shirt. If the person hasn’t specified a favorite color, they get whatever color the company currently has the most of.

One solution to this is 

```rust
#[derive(Debug, PartialEq, Copy, Clone)]
enum ShirtColor {
    Red,
    Blue,
}

struct Inventory {
    shirts: Vec<ShirtColor>,
}

impl Inventory {
    fn giveaway(&self, user_preference: Option<ShirtColor>) -> ShirtColor {
        user_preference.unwrap_or_else(|| self.most_stocked())
    }

    fn most_stocked(&self) -> ShirtColor {
        let mut num_red = 0;
        let mut num_blue = 0;

        for color in &self.shirts {
            match color {
                ShirtColor::Red => num_red += 1,
                ShirtColor::Blue => num_blue += 1,
            }
        }
        if num_red > num_blue {
            ShirtColor::Red
        } else {
            ShirtColor::Blue
        }
    }
}

fn main() {
    let store = Inventory {
        shirts: vec![ShirtColor::Blue, ShirtColor::Red, ShirtColor::Blue],
    };

    let user_pref1 = Some(ShirtColor::Red);
    let giveaway1 = store.giveaway(user_pref1);
    println!(
        "The user with preference {:?} gets {:?}",
        user_pref1, giveaway1
    );

    let user_pref2 = None;
    let giveaway2 = store.giveaway(user_pref2);
    println!(
        "The user with preference {:?} gets {:?}",
        user_pref2, giveaway2
    );
}
```

In this implementation we focused on closures, as such `store`, defined in `main`, has two blue shirt and one red. We call the `giveaway` method for both users with and without a preference.

>In the `giveaway` method, we get the user preference as a parameter of type `Option<ShirtColor>` and call the `unwrap_or_else` method on `user_preference`. The [`unwrap_or_else` method on `Option<T>`](https://doc.rust-lang.org/std/option/enum.Option.html#method.unwrap_or_else) is defined by the standard library. It takes one argument: a closure without any arguments that returns a value `T` (the same type stored in the `Some` variant of the `Option<T>`, in this case `ShirtColor`). If the `Option<T>` is the `Some` variant, `unwrap_or_else` returns the value from within the `Some`. If the `Option<T>` is the `None` variant, `unwrap_or_else` calls the closure and returns the value returned by the closure.
>
>We specify the closure expression `|| self.most_stocked()` as the argument to `unwrap_or_else`. This is a closure that takes no parameters itself (if the closure had parameters, they would appear between the two vertical bars). The body of the closure calls `self.most_stocked()`. We’re defining the closure here, and the implementation of `unwrap_or_else` will evaluate the closure later if the result is needed.

##### Closure Type Inference and Annotation

Another difference between closures and functions is that closures don't require type annotations or a return value. This is because closures are stores inside variables and are used without names which exposes them to users of the library.

In this way closures are usually short and are relevant only within a narrow context. Because of this limited context the compiler can infer the return type. 

Type annotations may be added to increase clarity and explicitness, though it may lead your code to being unnecessarily verbose. It would look like this:

```rust
let expensive_closure = |num: u32| -> u32 {
	println!("calculating slowly...");
	thread::sleep(Duration::from_secs(2));
	num
};
```

With the type annotation the closure syntax looks more similar to that of functions, here is an example of a function that adds 1 to its parameter, and its closure equivalent.

```rust
fn  add_one_v1   (x: u32) -> u32 { x + 1 }
let add_one_v2 = |x: u32| -> u32 { x + 1 };
let add_one_v3 = |x|             { x + 1 };
let add_one_v4 = |x|               x + 1  ;
```

All of the above closure definitions are valid, the only difference is the degree of explicitness and clarity in the code.

When not explicitly annotated the compiler assigns the closure the type inferred form its first use. So if a closure is used to handle strings earlier on in the program and then tries to handle an integer it will fail.

```rust
let example_closure = |x| x;

let s = example_closure(String::from("hello")); // passes
let n = example_closure(5); // fails
```

##### Capturing References or Moving Ownership

Closures can captures values in three ways, mimicking the ways a function can take a parameter, borrowing immutably/mutably and taking ownership. 

