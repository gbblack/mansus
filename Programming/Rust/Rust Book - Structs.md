
A *struct* (structure) is the same thing as a custom *type* in Typescript.
Or a data model for a database.
We can also think of a struct as a pattern to match an object's data attributes.

##### Defining and Instantiating Structs

Structs are similar tuples but are different in that you name each data type to give them more context/understanding. Since the data has names they no longer need to be in a specific order.

The struct is initialised using the `struct` keyword:

```rust
struct User {
	active: bool,
	username: String,
	email: String,
	sign_in_count: u64
}
```

To use a struct when created i.e. create an instance, we need to state each of the fields in the original struct and then give concrete values that match the type for each field. We don't need to specify in the same order.

```rust
fn main() {
    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1
    };
}
```

To access these values we use dot notation so `user1.active` will return `true`.
We can also change a value by assigning new values in dot notation: `user.active = false;`

Since structs work similarly to types when used in a function we can define a parameter or return type as being an instance of a struct.

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username: username,
        email: email,
        sign_in_count: 1,
    }
}
```

if a function parameter and field name are the same you can use this shorthand:

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

in this case username and email are both fields and parameters but rust will know to pass the parameter values into the same name field.

##### Creating Instances from Other Instances with Struct Update Syntax

Often you'll need to create a new instance of a struct that carries over some data from an old one. This is done by creating a new instance and only specifying the new/updated values while using the `..` operator to denote all else is the same as a previous struct.

```rust
fn main() {
    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}
```

The new information must come before the old if using this syntax.

When using structs data is moving ownership not referencing. If a struct contains non trivial types, such as String or another struct then all of these complex types must be moved to keep the original struct valid. In this case user1 is invalid as only email was updated. If both email and username were updated user1 would still be valid since its other fields are simple data types and are thus stored on the stack and have the `Copy` trait, i.e. a new identical copy of user1 while be made with new ownership containing all the old data that has been copied over, which os not possible for types stored on the heap.

##### Using Tuple Structs Without Named Fields to Create Different Types

There are a special kind of struct called *tuple structs*, tuple structs have the added meaning of a name for the structure but no names for the fields.
These are best used when you want a name for the tuple but giving names to the fields would be too redundant.

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

You can use index notation to access individual values: `black[0] // will give you 0`.

##### Unit-Like Structs Without Any Fields

A struct without a field is called a *unit like struct*. These are useful for when you need to apply a [[Rust Book - Traits|trait]] to some type but do not know the data you'd like top store. 

```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

Since the struct is empty there's no need for `{}`.

##### Ownership of Struct Data

We want structs to own all of their data so it is best to use `String` over `&str` for the fields as it keeps the ownership belonging to each struct instance. If you try to give a field the type string slice you'll get a compiler error because you have not specified a [[Rust Book - Lifetimes|lifetime]].

When using structs that you plan to reuse pass a reference to the struct into functions and so that the function that needs to use the struct can keep its ownership.