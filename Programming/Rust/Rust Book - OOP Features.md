Object-oriented programming (OOP) is a way of modelling programs.

##### Characteristics of Object-Oriented Languages

There is no consensus on ewhat features are required to make it be OOP, or any other paradigm, but there are certain common characteristics, namely objects, [[encapsulation]], and [[inheritance]] that keep appearing.

According to The Gang of Four (Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides) who wrote the Bible of OOP, _Design Patterns: Elements of Reusable Object-Oriented Software_ defined it in this way:

>Object-oriented programs are made up of objects. An _object_ packages both data and the procedures that operate on that data. The procedures are typically called _methods_ or _operations_.

With this definition Rust is object-oriented. Structs and enums have data and `impl` blocks provide methods for those structs and enums. They aren't called objects but provide the same functionality.

##### Encapsulation that Hides Implementation Details

Another aspect of OOP is _encapsulation_, which means that the implementation details of an object are NOT accessible to code using that object. Therefore the only way to interact with an object id through its public API, i.e. code using the object shouldn't be able to reach into the object to change its data or behaviour.

We learned to control encapsulation [[Rust Book - Managing Projects|here]]. We can use the `pub` keyword to decide which modules, types, functions and methods in our code should be apart of this public API. By default everything else is private.

```rust
pub struct AveragedCollection {
    list: Vec<i32>,
    average: f64,
}

impl AveragedCollection {
    pub fn add(&mut self, value: i32) {
        self.list.push(value);
        self.update_average();
    }

    pub fn remove(&mut self) -> Option<i32> {
        let result = self.list.pop();
        match result {
            Some(value) => {
                self.update_average();
                Some(value)
            }
            None => None,
        }
    }

    pub fn average(&self) -> f64 {
        self.average
    }

    fn update_average(&mut self) {
        let total: i32 = self.list.iter().sum();
        self.average = total as f64 / self.list.len() as f64;
    }
}
```

In this example the struct itself is public so it can be used elsewhere but the fields themselves are private so they can't be changed directly. To change those values we instead use an `impl` block to define behaviour that we want the struct to perform, making only those behaviours we want others to access public. So we want others to be able to add and remove values form the list field so those functions are made public but we don't want them to update the average directly so that is left to the default private status.

This encapsulation also allows for easier maintenance and versioning in the future as  we can add fields or behaviours and so long as we don't change the existing access value nothing else should change in our existing code. We could even change the type of the private fields without causing a compile so long as it behaves in the same way since no outside code touches those values directly and therefore do not care about its details.

##### Inheritance as a Type System and as Code Sharing

_Inheritance_ is a mechanism where an object can inherit elements from another object's definition, therefore inheriting the parents data and behaviour without needing to define them again.

Rust does NOT have this feature, there is no way to define a struct so as to receive another's definition.

You can mimic the functionality of not reusing code through traits as seen in [[Rust Book - Traits|here]].
We can also use traits to override implementations, like a child overriding a parent behaviour.

Another strength of inheritance is polymorphism, which is the idea that you can sap multiple objects with each other at runtime so long as they share certain characteristics.

##### Using Trait Objects That Allow for Values of Different Types

In [[Rust Book - Common Collections|collections]] we noted that one limitation of vectors is that they can only store elements of one type. A workaround we already covered is passing an [[Rust Book - Enums|enum]] into the Vector as its element type and have that enum contain options of differing types. This is great if we know all potential variants that might be used in the vector. 

> [!info]
> While structs, enums and traits all resemble objects as defined in other languages they critically don't combine data with behaviour. A struct and an enum hold data and a trait holds behaviour. We can also define behaviour in an `impl` block, which is still separate from structs and enums.

However if we aren't certain of the kinds of values that will be paced into a vector we can instead create a trait to define shared behaviour we expect to see from all the elements in the vector and pass that trait into the vector as its type. This way any object that implements this trait can be passed into the vector.

Using a trait object uses a special syntax where the type passed in is: `Box<dyn Trait>`.

```rust
pub trait Draw {       // trait defining behaviour
    fn draw(&self);    
}

pub struct Screen {   // a struct with a vec passing in that trait
    pub components: Vec<Box<dyn Draw>>,
}
```

This id different from using with a generic that uses [[trait bounds]].

[don't get it need more help](https://doc.rust-lang.org/book/ch17-02-trait-objects.html)

