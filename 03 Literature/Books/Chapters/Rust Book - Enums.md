Enums also known as *enumerations* are a type defined by enumerating its possible variants. Think of it like a set of states where only one state can be assigned at one time.

##### Defining an Enum

Enums are defined using the `enum` keyword along with a name and curly brackets.

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

To create an instance of an enum type we use `::` syntax.

```rust
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;
```

Enums can also include type information to keep things more concise.

```rust
enum IpAddr {
	V4(String),
	V6(String),
}

let home = IpAddr::V4(String::from("127.0.0.1"));

let loopback = IpAddr::V6(String::from("::1"));
```

As you can see above as part of the implementation of `enum` each state becomes a function that constructs its own instance. 

Enums states can also have varying types and associated data while still being a part of the same `enum` block.

```rust
enum IpAddr {
	V4(u8, u8, u8, u8),
	V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);

let loopback = IpAddr::V6(String::from("::1"));
```

Here's a more complicated example

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

This is equivalent to writing out 4 different structs, one for each state. The choice between one or the other is a design choice the the types are nearly entirely interchangeable.

You can also use the `impl` block to define methods for an `enum`.

```rust
impl Message {
	fn call(&self) {
		// method body would be defined here
	}
}

let m = Message::Write(String::from("hello"));
m.call();
```

##### The `Option` Enum And Its Advantages Over Null Values

In rust there is no `null` type. Instead we have the enum `Option`.  The Option enum defined only two states, a value can be something or it is nothing. 

Doing things this way means that all states are accounted for, including the nothing state, so if you function doesn't handle that possibility the compiler will pick it up and notify you. 

```rust
enum Option<T> {
    None,
    Some(T),
}
```

The `<T>` syntax means that the parameter is generic, i.e. it can be anything.

If you want a value to be null you have to explicitly use the Option enum.