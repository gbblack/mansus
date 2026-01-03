---
kind: chapter
tags:
---
[[Unit Testing]]
# The anatomy of a unit test

> [!abstract] Summary
> A breakdown of the AAA structure for writing tests, how to use text fixtures, guidelines for naming tests and using parametrized tests to keep the test suite concise.
## Note
---

> [!tip]- Table of contents
> 1. [[#How to structure a unit test]]
> 	1. [[#Using the AAA pattern]]
> 	2. [[#Avoid multiple arrange, act, and assert sections]]
> 	3. [[#Avoid if statements in tests]]
> 	4. [[#How large should each section be?]]
> 		1. [[#The arrange section is the largest]]
> 		2. [[#Watch out for act sections that are larger than a single line many assertions should the assert section hold?]]
> 	5. [[#What about the teardown phase?]]
> 	6. [[#Differentiating the system under test]]
> 	7. [[#Dropping the arrange. act and assert comments from tests]]
> 2. [[#Exploring the xUnit testing framework]]
> 3. [[#Reusing test fixtures between tests]]
> 	1. [[#High coupling between tests is an anti-pattern]]
> 	2. [[#The use of constructors in tests diminishes test readability]]
> 	3. [[#A better way to reuse test fixtures]]
> 4. [[#Naming a unit test]]
> 	4. [[#Unit test naming guidelines]]
> 	5. [[#Example: Renaming a test toward the guidelines]]
> 5. [[#Refactoring to parametrized tests]]
> 	1. [[#Generating data for parametrized tests]]
> 6. [[#Using an assertion library to further improve test readability]]

### How to structure a unit test
#### Using the AAA pattern
- The AAA pattern stands for: Arrange, Act, Assert.
- It is a simple and uniform structure that can be applied to all tests.
- After gaining some skill in this structure tests become easier to read and maintain as they all follow this format uniformly.
- Arrange: Bringing the SUT and its dependencies into a desired state.
- Act: calling methods on the SUT, passing dependencies and capturing output.
- Assert: Verifying the outcome.
- Another similar structure is Given-When-Then where: Given -> Arrange, When -> Act, Then -> Assert
- The only meaningful distinction between the two is that Given-When-Then is more readable for non-programmers.
- When writing a test you can using TDD you start with the assert section since you haven't implemented any behaviour to be arranged or acted on yet.
- If production code already exists its better to start with the arrange section.
#### Avoid multiple arrange, act, and assert sections
- Avoid multiple act or assert sections, as the test now verifies multiple behaviours which changes it from a unit test to an integration test as per our definition from [[Unit Testing-02#Integration tests in the two schools|Chapter 2]].
![[UnitTesting-013.png]]
- To avoid this make sure the unit test covers only a single action and assertion.
- This keeps tests simple and clear.
- Integration tests may have multiple act sections as a way to speed up a test suite but do not make this optimization if the integrations test are already fast.
#### Avoid if statements in tests
- `if` blocks in unit tests are an anti-pattern. Each test should cover a simple set of steps, no branching.
- An `if` block should be split into several tests, one for each outcome. Also true for integration tests.
#### How large should each section be?
##### The arrange section is the largest
- The arrange section is usually the largest.
- To keep it from getting out of controls there are two pattern we can use: Object Mother and Test Data Builder. [[chapter 10]]
##### Watch out for act sections that are larger than a single line
- The act section should only be a single line of code.
- Any more indicates a problem with the design of the underlying code.
- The SUT should be designed in such a way that every unit of behaviour can be achieved with a single call.
- Example: A successful purchase has two outcomes, the customer gains an item and the store loses the same one.
  A method call need to encapsulate both states to prevent inconsistencies. This inconsistency is called [[invariant violation]]. The act of protection against this is [[encapsulation]].
- The guideline of keeping act as one line can help guarantee the encapsulation of all business behaviour.
- An exception to this guideline is for infrastructure or utility code.
#### How many assertions should the assert section hold?
- The common wisdom is there should be one assertion per test. This is from the idea that a unit test covers a unit of code, not a unit of behaviour.
- But with out understanding of a unit test being a unit of behaviour you can have as many assertions as there are outcomes from that behaviour.
- However avoid large assertion blocks as they can be a sign of a missing abstraction in the underlying code.
- Example: Instead of asserting all properties in an object, define a proper equality member in the object class and assert once against that.
#### What about the teardown phase?
- The teardown section is functionality that returns the test to its original state: removing files, closing database connections, … .
- It is reused across tests and thus isn't part of the AAA pattern.
- Most unit tests do not need teardown, as they shouldn't leave side effects that need to be removed.
- Teardowns are more relevant for integration tests.
#### Differentiating the system under test
- There should only be one point of entry in the SUT which is what the test uses to trigger the behaviour we are testing.
- Therefore its important to differentiate the SUT from its dependencies in the test.
- A good way to do this is by capturing the outcome from the act call in a variable named `sut`.
#### Dropping the arrange, act, and assert comments from tests
- The AAA section should be differentiated from each other for better readability and maintainability.
- A common way it to include single line comments above each section: `// arrange`. Good for larger tests.
- If the test is mall having an empty line between the sections is enough.
### Exploring the xUnit testing framework
- Test shouldn't be a dull enumeration of product code, but instead provide a high level description of the application's behaviour.
- Frameworks help enable this description.
### Reusing test fixtures between tests
- Reusing code between tests is essential for keeping them short and simple.
> [!info]- Test Fixture
> A test fixture is an object that test runs against. It can be anything so long as it stays in a known fixed state.
- An incorrect way to use test fixtures is by initialising them in the test constructor.
- While this greatly reduced the amount of test code it has two major drawbacks:
	- introduces high coupling between tests.
	- diminishes test readability.
#### High coupling between tests is an anti-pattern
- If the fixture is passed into the constructor then a change in one test will occur in another.
- This violates an important guideline: a modification of one test should not affect other tests.
- This is related to the tests being in isolation from each other.
#### The use of constructors in tests diminishes test readability
- Arrangement in the constructor reduces test readability, you cannot see the full picture just by looking at the test.
- This can generate uncertainty on failure in where the fault lies: in instantiation? or the test itself?
#### A better way to reuse test fixtures
- The way to reuse test fixtures is through private factory methods from the test class.
- Extract common initialization code into a private method so as to keep the full context of what's happening in the tests. This also stops the test being highly coupled.
- Keep these methods generic to allow the tests to specify how they want fixtures to be created.
- One exception: a fixture can be instantiated in the constructor if its used by almost or all of the tests.
- Most often seen in integration tests, such as a database connection.
### Naming a unit test
- Naming the test well is important as it helps you understand what the test is verifying and how the SUT behaves.
- Keep the names in natural, simple English.
- That way you can describe the system behaviour in a meaningful, readable way, even to a non-technical audience.
- Not doing this adds a cognitive load for understand the test suite that builds up over time and thus increases maintenance cost. Especially after a long time or when reading someone else's code.
#### Unit test naming guidelines
- Naming guidelines:
	- Don't follow a naming policy, too rigid.
	- Name a test as if you were describing the problem to a non-programmer.
	- Separate words with underscores, improves readability.
#### Example: Renaming a test toward the guidelines
- Don't include the name of the SUT in the test name.
- We're testing behaviour and not code so tying the test name to the SUT like that means that if you change the underlying code you'll also need to change the test name, this is harder to maintain.
- The only exception is for utility code.
- The word should is an anti-pattern. We are stating facts not uncertainties.
### Refactoring to parametrized tests
- One test is likely not enough to describe a unit of behaviour.
- To keep the test suite from growing too large to control we can group similar tests using [[parametrized tests]].
![[UnitTesting-014.png]]
- Parametrized test are an option offered by frameworks.
- They improve the test suite as it reduces the amount of code needed to be maintained while only making it slightly harder to read.
#### Generating data for parametrized tests
- The data for these parametrized test must be values the compiler understands and none that rely on runtime.
- This can be overcome with a method that generates the data that the parametrized tests can accept.
### Using an assertion library to further improve test readability
- Using an assertion library can help readability. They change how you write assertions to be more readable.
- They also provide many helper methods to improve your test suite.

Next: [[Unit Testing-04]]
Previous: [[Unit Testing-02]]
## Citation
---

```
V. Khorikov, "The anatomy of a unit test" in *Unit Testing: Principles, Practices, and Patterns*, 1st. M. Stephens, M. Michaels, S. Zaydel, A. Dragosavljevic, A. Calcara, T. Taylor, F. Buran, Eds. Shelter Island, NY, USA: Manning, 2020, pp. 41-67.
```