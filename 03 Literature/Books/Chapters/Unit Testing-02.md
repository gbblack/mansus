---
tags:
  - type/chapter
  - status/day
  - classical_testing
  - unit_testing
created at: 2025-02-15
---
[[Unit Testing]]
# What is a unit test?

> [!abstract] Summary
> A breakdown of the London and Classical schools of testing and their differences. Navigates the benefits of both sides with the Classical approach winning as the better one for sustainable software growth. Includes precise criteria for unit testing and isolation under this approach.
## Note
---
> [!tip]- Table of Contents
> 1. [[#The definition of "unit test"]]
> 	1. [[#The Isolation issue: The London take]]
> 	2. [[#The Isolation issue: The classical take]]
> 2. [[#The classical and London schools of unit testing]]
> 	1. [[#How the classical and London schools handle dependencies]]
> 3. [[#Contrasting the classical and London schools of unit testing]]
> 	1. [[#Unit testing one class at a time]]
> 	2. [[#Unit testing a large graph of interconnected classes]]
> 	3. [[#Revealing the precise bug location]]
> 	4. [[#Other differences between the classical and London schools]]
> 4. [[#Integration tests in the two schools]]
> 	1. [[#End-to-end tests are a subset of integration tests]]

### The definition of "unit test"
- A unit test is an automated test that:
	1. Verifies a small piece of code (a unit).
	2. Does it quickly.
	3. Does it in an isolated manner.
- The isolation criteria is the root of the difference between the classical and London schools of unit testing. They disagree on what isolation means.
#### The Isolation issue: The London take
- The London school describes isolation as isolation of the underlying code from its collaborators.
- This means any dependencies, such as other classes or methods, need to be replaced by a test double.
- The idea is that this way you can focus just on the code under test and nothing else.
- A test double is an object that looks and behaves like its counterpart but is actually a simplified version that facilitates testing.
![[UnitTesting-007.png]]
- A benefit of this is that if a test fails you know exactly where in the codebase the problem is occurring.
- Another benefit is the ability to split the object graph, which is the web of communicating classes solving the problem.
- This graph can grow to be very complex, where each class has several dependencies who each have their own and so on.
- To test such an interconnected codebase is difficult without test doubles. Since they can be used to substitute the immediate dependencies of the code under test it breaks up this object graph.
- This substitution also allows for a guideline to test only one class at a time which keeps the unit testing structure simple. It reduces the decision of what to test as: Have a class? Test it.
![[UnitTesting-008.png]]

> [!info]- Definitions
> - MUT: method under test.
> - SUT: system under test:
> - Test Double: an overarching term that describes all fake dependencies in a test.
> - Mock: a mock is one kind of fake dependency, specifically one that allows for the examination of interactions between the SUT and its collaborators.

> [!note]- Interfaces and mocks
> Mocking against an interfaces is required for isolating the SUT from its collaborators. mocking concrete classes is an anti pattern, more **CHAPTER 11**.

- The assertion in this kind of test does not look at the classes changed state, it can't since its using mocks, instead it checks to see if the SUT made the right calls to the mock class.

#### The Isolation issue: The classical take
- In the London approach a small piece of code, unit, can only ever get as big as a class, anything else can't be isolated and is therefore too big.
- In the classical approach its not the code that should be isolated but rather that the unit tests should be in isolation from each other.
- This way tests can be run in parallel, or any other way as they won't affect each other's outcome.
- This means several classes can be tested at once so long as they don't share in any state.
- A shared state would be something like a database or file system.

> [!info]- Definitions
> - Shared dependency: is a dependency that is shared between tests and provides ways for tests to affect each other's outcomes.
> - Private dependency: is a dependency that is not shared.
> - Out-of-process dependency: is a dependency that runs outside the application's executable, such as an external library. Often these are also shared dependencies.

- The classical school uses less mocks and test doubles, these are usually only used to substitute dependencies that introduce a shared state.
- Shared dependencies are shared between unit tests, not classes under test, knowing this a singleton dependency is not shared so long as a new one is created for every test.
- An example: a configuration class which is reused across all production code but is injected anew into the SUT for every test then. This way the dependency is neither mocked or shared as each test uses their own individual new instance of the configuration.
![[UnitTesting-009.png]]
- There exists another kind of dependency: a *volatile dependency*.
- A volatile dependency is a dependency with the following properties:
	- It introduces a requirement to set up and configure a runtime environment in addition to what is on the developer's machine by default. Consider Databases or API services.
	- It contains non-deterministic behaviour. Think of a random number generator class, or a function returning the current date. These dependencies produce different results each time they are invoked.
- There can be overlap between volatile and shared dependencies.
- Another reason to substitute shared dependencies in the classical approach is that it increases execution speeds.
- Knowing all this we push testing shared dependencies outside of the unit test and into the integration test.
- All this shows that the classical school has a different definition of what is a unit of code is for their tests. In this school a unit can be a single class or many, so long as none of them has a shared dependency.
### The classical and London schools of unit testing
- The difference between the two schools is on the isolation issue:
	- London: is isolation of the SUT from its collaborators.
	- Classical: is isolation of the unit tests from each other.
- To sum up the disagreement we can break it down to three factors:
	1. The isolation requirement.
	2. What constitutes a "unit" of code under test.
	3. Handling dependencies.

|           | isolation of | A *unit* is               | Uses test doubles for          |
| --------- | ------------ | ------------------------- | ------------------------------ |
| London    | Units        | A class                   | All but immutable dependencies |
| Classical | Unit tests   | A class or set of classes | Shared dependencies            |

#### How the classical and London schools handle dependencies
- London school allows for immutable dependencies to be passed in. Only elements that can NOT be modified need to be substituted.
- These immutable objects are called value objects or values, these are defined by their content and have individual identity.
- An example: the integer `5`, it doesn't matter how many instances there are they are all interchangeable.
- A dependency can be either shared or private.
- And a private dependency can be either mutable or immutable.
![[UnitTesting-010.png]]
Definition: A collaborator is a dependency that's either shared or mutable.
- Not all out-of-process dependencies are shared. A shared dependency almost always lives outside the app's process but the opposite is not true.
- For out-of-process dependencies to be shared it needs for there to be a mean of the unit tests communicating.
- This communication id done through modification of the dependency's internal state.
- With this understanding an immutable out-of-process dependency doesn't allow for a change of state and is therefore not shared.
![[UnitTesting-011.png]]

### Contrasting the classical and London schools of unit testing
- Again the main difference between he two schools is how they treat isolation.
- The classical school is preferred because it better supports the goal of sustainable software growth.
- Test with mocks are inherently more fragile and brittle  which can work against the stated goal of software growth.
- There are still benefits to the London school:
	- Better granularity, test are focused on only one class at a time.
	- Easier to test an interconnected class since its graph is broken up by test doubles.
	- On failure you know exactly what underlying code has failed.
#### Unit testing one class at a time
- What is a unit? The London school thinks a unit is a class.
- However this is misleading as tests should verify behaviour not code. The number of classes needed to implement a behaviour should be irrelevant.
- So long as a test checks a single unit of behaviour it is a good test.
- A test should tell a story about the problem your code solves and this story should be cohesive and meaningful to a non-programmer.
#### Unit testing a large graph of interconnected classes
- Mocks can help make it easier to test a class as it breaks up the dependency chain which can reduce the initial setup.
- This means that the absence of micks in the classical approach requires more of an elaborate setup.
- This trade off however focuses on the wrong problem. Instead of mocks being used to break up the complex object graph, the underlying code should be written in such a way as to not create such a graph in the first place.
- Large object graphs are a code design problem, not a testing one.
- Classical tests are great for finding negative behaviour as it exposes the the problem in the underlying code design, mocks meanwhile just hide it.
#### Revealing the precise bug location
- With the classical approach if a bug is introduced then there is a danger if a ripple effect across the test suite where all those classes relying on the failing one will also fail.
- This makes it harder to identify which class is actually hosting the breaking bug.
- However this issue is mitigated by the fact that unit tests should be run regularly, ideally after every change (commit). This way you need only investigate the small code change you made.
- Another bonus is that cascading test failure indicates that a piece of code of of great value to the entire system, which is good to know.
#### Other differences between the classical and London schools
- Two more differences between the two schools:
	- Approach to TDD.
	- The issue of overspecification.

> [!info]- TDD: Test Driven Development
> TDD is a software development process that relies on tests to drive project development. Consists of three stages for every test case:
> 1. Write a failing test where functionality needs to be added.
> 2. Write enough code for the test to pass.
> 3. Refactor the code with the protection of the passing test to be made more readable and maintainable.
- The London school leads to outside-in TDD, where you start from higher-level tests.
- By using mocks you can specify which collaborators the system should communicate with to achieve the expected results.
- You then implement your way through the mocked classes.
- The classical approach doesn't have this same guidance since you need to use the real objects. Instead an inside-out approach is used.
- This is done by starting with the domain model and adding layers on tip until the software becomes usable by the user.
- A more crucial distinction is that of over specification, i.e. coupling the test to the implementation details. The London school produces tests where this is more likely to occur.
- This point of tight coupling is the main objection against the London school and their ubiquitous use of mocks.
### Integration tests in the two schools
- The London and classical schools also diverge on integration testing.
- According to the London school most classical school unit tests would actually be integration tests.
- A such here is a revised definition of a unit test, according to the classical school:
	- Verifies a single unit of behaviour.
	- Does it quickly.
	- does it in an isolated manner from other tests.
- An integration test is one that doesn't meet all of this criteria.
- An example: test that reach out to a shared dependency, i.e. a database.
- These tests would need to run sequentially to not affect each other making it slow, failing the second criteria.
- A test also becomes an integration test if it is verifying two or more units of behaviour.
- This includes verifying how two or more modules, developed separately, work together.
#### End-to-end tests are a subset of integration tests
- An integration test is a test that verifies your code is working in *integration* with a shared or out-of-process dependency or code worked on by other's.
- End-to-end (E2E) tests are a subset of integration tests, the difference is that E2E tests include many more of these shared dependencies.
- Their name indicates their function, a test to verify the system from the end user's point of view.
- They can also be know as UI, GUI or functional tests.
- An example: Let's say there are three out-of-process dependencies: a database, a file system and a payment gateway.
- An integration test would include the database and file system  and test double the payment gateway.
- This is because you have control over the first two dependencies but not the last.
![[UnitTesting-012.png]]
- The E2E test will cover all three dependencies, regardless of control. As such E2E tests are run only sparingly and only after all unit and integration tests have passed. 
- Sometimes E2E tests are only run on build and not locally.

Next: [[Unit Testing-03]]
Previous: [[Unit Testing-01]]
## Citation
---
```
V. Khorikov, "What is a unit test?" in *Unit Testing: Principles, Practices, and Patterns*, 1st. M. Stephens, M. Michaels, S. Zaydel, A. Dragosavljevic, A. Calcara, T. Taylor, F. Buran, Eds. Shelter Island, NY, USA: Manning, 2020, pp. 20-40.
```