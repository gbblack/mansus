Methods are declared with the `fn` keyword and a name. They optionally can have parameters, a return type and a body with some code.

Methods are different from functions in that they are defined within a struct, enum or trait object. They are also different in that their first parameter must always be `self`.

`self` is the instance of the struct the method is being called on.

Think of methods as behaviours the struct should be able to do, or methods in a class.
##### Defining Methods

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```

When defining a method we first use the keyword `impl` followed by the struct name, every method defined in the `impl` block will they be associated with the same name struct. 

When using the `self` parameter use it as you would if you were referencing the struct itself, because you are. As such is best to borrow self so that ownership stays with the referenced struct.

Now when you want to use the method you can use dot notation just like with fields, only you include the parameters (if any), the self parameter is implicit and does not get explicitly passed through. 

The main purpose of methods is for organisation, this way we can keep all information and behaviour of a struct in one place.

We can also use these methods for traditional getter and setter functions as methods can have the same name as fields. 

```rust
impl Rectangle {
    fn width(&self) -> bool {
        self.width > 0
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    if rect1.width() {
        println!("The rectangle has a nonzero width; it is {}", rect1.width);
    }
}
```

##### Associated Functions

All methods defined in an `impl` block are called *associated functions* because they are associated with a struct. We can have associated functions that are NOT methods (they do not need an instance of a struct), as they do not have the `self` parameter.

These functions are used mostly for constructors that will return a new instance of a struct. Convention is to call them `new` but that is not required and new is NOT a keyword in rust.

An example for the rectangle struct constructing a square (so we don't need to repeat the parameter twice).

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}
```

The `Self` keyword are aliases for the struct named in the `impl` block.

To call an associated function we use the `::` syntax with the struct name `let sq = Rectangle::square(3);` as an example. This syntax is also used for namespaces created bny [[modules]].

##### Multiple `impl` Blocks

Every struct is allowed many `impl` blocks, where this is useful is in [[Rust Book - Generics|Chapter 10 of the Rust Book]].
