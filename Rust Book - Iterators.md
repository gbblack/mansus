A significant influence on Rust is [[functional programming]]. To program in a functional style we use functions as values passed into other functions, return function, assign functions to variables and more.

Rust share some of this functionality through these features:

- Closures: a function like construct to store a variable
- Iterators: a way of processing a series of elements

### Iterators

In Rust an iterator is the trait `Iterator` applied over a collection. It requires only that a method for `next` be defined.
When using the `for` construct this happens automatically as it internally uses the `.into_iter()` method.

```rust
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;

    // methods with default implementations elided
}
```

Iterators use `Self::Item` which is an [[Chapter 19|associated type]].

We can also call the next method explicitly, but it requires the variable holding it to be immutable:

```rust
#[test]
fn iterator_demonstration() {
	let v1 = vec![1, 2, 3];

	let mut v1_iter = v1.iter();

	assert_eq!(v1_iter.next(), Some(&1));
	assert_eq!(v1_iter.next(), Some(&2));
	assert_eq!(v1_iter.next(), Some(&3));
	assert_eq!(v1_iter.next(), None);
}
```

##### Methods that Consume the Iterator

The `Iterator` trait has several default implementations in the standard library. Some of them call the `next` method in their definitions which is why you need to define it in your implementation as well.

Methods that call `next` are called [[consuming adaptors]] because to call them uses up the iterator, i.e. once called whatever variable now owns that iterator and it can't be called again.

##### Methods that Produce Other Iterators

[[Iterator adaptors]] are methods defined on the `Iterator` trait that don't consume it, instead they produce a new one that shares some aspects of the original.

An example of one of these is the method `map` which takes a closure to call on each item that is iterated upon and returns a new iterator with the modified item. 

##### Using Closures that Capture Their Environment

Many [[Iterator adaptors]] take closures as arguments and can commonly be closures that capture their environment.

An example id the method `filter`: it work by a  closure getting an item from an iterator and returning a `boolean`. If the return value is `true` the value will be included in the iteration and if it returns `false` it will not be included.

