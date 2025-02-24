---
tags:
  - type/article
  - testing
  - status/day
author:
  - Hillel Wayne
publication:
  - Computer Things
source: https://buttondown.com/hillelwayne/archive/some-tests-are-stronger-than-others/
created: 2025-02-24
---
# Some tests are stronger than others

> [!abstract] Summary
> The author compares the idea of property strength from formal methods to testing strength in programming. With this idea you can think of "stronger" tests being used for defining correctness in the program and "weaker" tests for localizing bugs on failure.
## Highlights
---
> In formal methods we have a notion of property strength, defined as `STRONG => WEAK`.

> ==**Any system that has STRONG as a property also has WEAK**==. This matches our notion of _strength_ because a system can have ==**a bug that breaks STRONG but not WEAK, but can't have a bug that breaks WEAK but not STRONG.**== In a sense, **WEAK is redundant, because it cannot give us any "new information" about correctness.**

> If `TEST1 => TEST2`, then there's no bug which TEST2 would catch that TEST1 does not.

> The simplest example would be:
> 
> `test A {add(1, 2) does_not_raise_error} test B {add(1, 2) > 1} test C {add(1, 2) == 3}`
> 
> `C => B => A`, as if `add(1, 2) == 3`, then it's definitely greater than one and definitely didn't raise an error.

> A more common and useful case is integration tests. **An integration test crossing modules A== and B will imply some unit tests in A and some unit tests on B.**==

> This is also demonstrates **that _weaker_ doesn't mean _not useful_**: it's easier to localize an error if a unit test fails than if an integration test fails.

> One cool thing we can do is take a property P and generate a stronger/weaker property _without knowing anything about P_

> An example with tests would be mock-removal:
> `test weak {f(x) calls mock M} test strong {f(x) == y}`

> If you _already know there's an error_, **a ==weaker test ==can be more useful than a stronger test, because it l==ocalizes where the bug is==** more. If you're **trying to ==_determine correctness==_, though, ==stronger tests== are better.**
## Citation
---
```
H. Wayne, "Some tests are stronger than others", Computer Things.
Available: https://buttondown.com/hillelwayne/archive/some-tests-are-stronger-than-others/
```