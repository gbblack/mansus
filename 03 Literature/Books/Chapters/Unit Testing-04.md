---
kind: chapter
tags:
---
[[Unit Testing]]
# The four pillars of a good unit test

> [!abstract] Summary
> Deep dive into defining the criteria of a good test: includes definitions, reasoning and strategies for where, when and how to read and write good tests.
## Note
---
> [!tip]- Table of Contents
> 1. [[#Diving into the four pillars of a good unit test]]
> 	1. [[#The first pillar: Protection against regressions]]
> 	2. [[#The second pillar: Resistance to refactoring]]
> 	3. [[#What causes false positives?]]
> 	4. [[#Aim at the end result instead of implementation details]]
> 2. [[#The intrinsic connection between the first two attributes]]
> 	1. [[#Maximizing test accuracy]]
> 	2. [[#The importance of false positives and false negatives: The dynamics]]
> 3. [[#The third and fourth pillars: Fast feedback and maintainability]]
> 4. [[#In search of an ideal test]]
> 	1. [[#Is it possible to create an ideal test?]]
> 	2. [[#Extreme case 1: End-to-end tests]]
> 	3. [[#Extreme case 2: Trivial tests]]
> 	4. [[#Extreme case 3: Brittle tests]]
> 	5. [[#In search for an ideal test: The results]]
> 5. [[#Exploring well-known test automation concepts]]
> 	1. [[#Breaking down the Test Pyramid]]
> 	2. [[#Choosing between black-box and white-box testing]]

### Diving into the four pillars of a good unit test
- A good unit test has four attributes:
	1. Protection against regressions.
	2. Resistance to refactoring.
	3. Fast feedback.
	4. Maintainability.
- These are foundational testing attributes and can be used to analyse unit, integration and end-to-end tests.
#### The first pillar: Protection against regressions
- A regression is a software bug. It's what happens after a code change breaks an existing feature.
- The chance for regressions grow as your software grows. Remember *code is not an asset, it's a liability*.
- Larger code bases provide larger surface area  for bugs.
- Protection against regressions is therefore essential to the sustainability of a project.
- To evaluate how a test scores on this metric we look at the following:
	- Amount of code executed during the test.
	- Complexity of the executed code.
	- The code's domain significance.
- Generally the more code that gets executed the higher the chance of revealing regressions.
- However it's not just amount that matters but complexity and domain significance.
- Bugs in complex business logic are much more damaging than ones found in boilerplate code.
- By this metric it's also not worth it to test trivial code, as there is too low a chance of finding a regression.

> [!example]- Trivial Code
> `public class User { public String Name {get; set;} }`

- External code such as libraries or frameworks also count towards code to protect against regressions. For a better test it must include these external code bases in the testing scope.
- This is to check the assumption your code makes on these libraries is correct.
#### The second pillar: Resistance to refactoring
- Resistance to refactoring is the degree to which a test can sustain refactoring of the underlying code without failing.

> [!note]- Definition: Refactoring
> Refactoring means changing existing code without modifying its observable behaviour. e.g. changing a method name.

- A situation where a test fails from a refactor that doesn't change functionality is called a *false positive*, AKA a false alarm.
- To evaluate a test on this metric you need to reduce the number of false positives it can generate.
- Why? Because a test string against refactoring allow for sustainable software growth by providing these two benefits:
	1. Tests provide an early warning when you break existing functionality.
	2. You become confident that your code changes don't lead to regressions.
- False positives interfere with both of these benefits:
	1. If a test fails for no good reason it reduces faith in the suite and you begin to ignore their warnings, causing legitimate failures to slip through.
	2. You lose confidence in the test suite, leading to fewer code changes in an effort to avoid regressions.
#### What causes false positives?
- False positives are related to a test's structure. The more tightly a test is tied to implementation details the more likely it is to generate false positives.
- The only way to reduce false positives is by decoupling the test from the underlying code.
- Tests must verify the SUT, its observable behaviour, not the steps it takes to get there.
- The best way to structure a test for this is by telling a story about the problem domain.
- Should a test fail then it indicates a disconnect between the story and application behaviour.
![[UnitTesting-015.png]]
- Tests that are tied to implementation details are NOT resistant to refactoring. They display both shortcomings mentioned above:
	- They don't provide an early warning, you've ignored them due to little relevance.
	- They hinder ability/willingness to refactor.
#### Aim at the end result instead of implementation details
- The only way to avoid brittleness in tests is to increase their resistance to refactoring, through decoupling.
- To do this:
	- Aim to verify the end result.
	- Treat each method under test as a black box, you only care about its output.
![[UnitTesting-016.png]]
- This won't cover all false positives as refactoring can lead to compilation errors, say adding a required parameter to the MUT, but these aren't a danger as they are easy to spot and fix.
### The intrinsic connection between the first two attributes
- There's an intrinsic connection between the first two attributes. They both contribute to the accuracy of the test.
- Protection against regressions is essential for development right away, the need for resistance to refactoring is not so immediate.
#### Maximizing test accuracy
- There are four possible outcomes for test results:
	- It can either pass or fail.
	- The functionality is either correct or broken.
- The situation where the test passes and the SUT works is the correct inference -> true negative.
- Another correct inference is the situation where the the SUT functionality is broken and the test fails -> true positive.
![[UnitTesting-017.png]]
- When a test passes but the functionality is broken -> false negative.
  This is what protection against regressions helps minimize.
- The final outcome is when the test fails but the SUT functionality is correct -> false positive.
  Resistance to regressions helps with this case.
- The lower the amount of false positives and false negatives in a test makes for a more accurate test.
- These first two attributes are all about the accuracy of a test.
- We can define accuracy like this:
	- How good a test is at indicating the presence of bugs -> no false negatives, domain of protection against regressions.
	- How good the test is at indicating the absence of bugs -> no false positives, domain of resistance to refactoring.
- We can think of accuracy like a ration between signal and noise. With this idea there are only two ways to increase the accuracy of a test:
	- First is to improve the signal, i.e. better at finding regressions.
	- Second is to reduce the noise, i.e. it raises no false alarms.
![[UnitTesting-018.png]]
#### The importance of false positives and false negatives: The dynamics
- In the short term false positives aren't as bad as false negatives, however as a product grows they have an increasing effect that can take over.
![[UnitTesting-019.png]]
- This is because refactoring becomes more important over time while being rare at the beginning of a project.
- As time goes on the codebase deteriorates, it becomes complex and disorganized.
- Refactors are required to keep that degradation in check, otherwise the cost of adding new features becomes prohibitive.
- Despite their importance many developers don't think of false positives in this way, usually they focus on the first attribute, protection against regressions.
- The reason is because far fewer projects reach that later stage. many projects stay small or simple and never grow too big.
- Nevertheless you should still pay attention to false positives in bigger projects.
### The third and fourth pillars: Fast feedback and maintainability
- The third and fourth pillars are:
	3. Fast Feedback
	4. Maintainability
- Tests that run quickly give fast feedback and can warn you about bugs as soon as they appear.
- Slow tests delay feedback and dissuade you from running them often, leading to more waste and bugs.
- The maintainability metric evaluates maintenance costs:
	- *How hard is it to understand the test?* This is related to the size of a test, smaller tests are more readable and easier to understand. (Tests are still *[[first-order citizen|first class citizens]]* in code and shouldn't be shortened through cut corners).
	- *How hard is it to run the test?* If the test relies on out of process dependencies then time is needed to keep them operational.
### In search of an ideal test
- When trying to find the value of a test we can multiply the values of each attribute together: attribute * attribute * … .
- This means that no attribute can be ignored entirely and given a score of 0, as by the rules of multiplication this will drag down the entire test to 0.
- As such each attribute must have a score in some way.
- There is no way actual way to measure attributes, this is a framework to be able to better evaluate tests.
- Remember: all code is a liability so only allow high scoring tests into the test suite.
- A small number of high scoring tests is more valuable that many more low scoring ones
#### Is it possible to create an ideal test?
- An ideal test is impossible.
- This is because the first two pillars, protection from regressions and resistance to refactoring, are mutually exclusive.
- So instead of trying to maximize them all, focus on making sure the remaining pillars don't fall too far behind.
- Of course remember, all pillars (attributes) must be represented, if one is missing the whole test fails.
#### Extreme case 1: End-to-end tests
- E2E test exercise a lot of code and therefore do great at the first pillar, protection against regressions.
- They are also immune to false positives as they are designed exclusively from an end user's perspective. This covers resistance to refactoring as E2E tests specifically only ever care about observable behaviour.
- The major drawback to E2E test is that they are slow. So slow that relying on them is not possible for most dev teams.
![[UnitTesting-020.png]]
#### Extreme case 2: Trivial tests
- Trivial tests cover such a simple piece of code that it is unlikely to ever break. e.g. one liners.
- These tests do provide fast feedback and are very resistant to refactoring.
- However they are unlikely to ever reveal a regression, they are simply too simple.
- These tests are also called *tautology tests*, usually seen to be set up in such a way as they will always pass or have meaningless assertions.
![[UnitTesting-021.png]]
#### Extreme case 3: Brittle tests
- Brittle tests are those that run quickly and are likely to catch regressions.
- However they are also are very likely to create false positives when refactoring.
- These kinds of tests are tightly coupled to implementation details of the underlying code.
- They focus on the *how* rather than the *what*, preventing easy refactoring.
![[UnitTesting-022.png]]
#### In search for an ideal test: The results
- It's impossible to create an ideal test with high scores in all pillars.
!UnitTesting-023.png
- The fourth pillar, maintainability, is not correlated to the other three, except for E2E tests which need to be large to support all the setup for the app's dependencies and therefore must have higher maintenance costs.
- Writing a good test is hard. It requires trade offs between pillars without any of them getting to 0.
- Any sacrifice made needs to be partial and strategic.
- Initially you might think that a good strategy is to concede a little on all the attributes.
- This is incorrect, in reality resistance to refactoring is non negotiable. All tests should aim for as much as possible of this attribute.
- The trade off then is between how fast the test runs and how good it is at catching bugs.
- Think of it as a slider, the more value in one pillar the less in the other.
- The reason resistance to refactoring is no negotiable is that whether or not a test has this attribute is binary, it does or it doesn't.
![[UnitTesting-024.png]]
> [!note]- Tip
> Eradicating brittleness from tests is the first priority in building a robust test suite.
### Exploring well-known test automation concepts
- Remember: These four pillars (attributes) are foundational to testing and all existing automation concepts can be traced back to them.
#### Breaking down the Test Pyramid
- The test pyramid is a concept that advocates for a certain ratio of different kinds of tests:
	- Unit tests
	- Integration tests
	- E2E tests
- The width of the pyramid refers to the prevalence of a kind of test in the suite. The wider the layer the greater the number.
- The height refers to how close the tests are to emulating the end user behaviour.
![[UnitTesting-025.png]]
- Different test pyramids make different trade offs between fast feedback and protection against regressions.
- Tests in higher layers prefer protection while lower ones prefer speed.
![[UnitTesting-026.png]]
- The mix of tests will be different for each team and project but in general the pyramid should follow the shape: Unit test majority, E2E minority, integration in the middle.
- E2E are low on fast feedback and maintainability, therefore they should only be applied to the most critical functionality, ones were we'd never want to see any bugs.
  And only if you can't guarantee this protection with unit and integration tests.
- There are exceptions, if your app only does basic read, update, create and delete operations with little business logic or complexity then your "pyramid" will look like a rectangle.
  Equal unit and integration tests with no E2E tests.
- Unit tests are also less valuable in an app without complex business logic or algorithms. They quickly descend into trivial territory.
- However it is always valuable to test how code works with other systems so integration tests may end up in greater number than unit tests.
- Another exception is an API that reaches out for a single out-of-process dependency, say a database, here E2E tests are a viable option. There is no GUI so they can still run quickly.
- In this case E2E tests are indistinguishable from integration tests.
#### Choosing between black-box and white-box testing
- Another test automation concept is black-box vs white-box testing:
	- *Black-box testing*: is a method that examines functionality without knowing internal structure, built around specs and requirements. What the app is supposed to do rather than how.
	- *White-box testing*: is the opposite, it verifies the internal workings of software, built from source code.
- White-box testing is more thorough but brittle, it tends to lead to tight coupling between test and implementation.
- Black-box provides the opposites pros and cons, less thorough but decoupled from implementation.
- Since we can't compromise on refactoring it's best to choose black-box testing over white-box by default.
- When writing all the tests, unit tests, integration tests E2E tests, view the system as a black-box and verify behaviour meaningful to the problem domain.
- If a test can't be traced back to a business requirement it is a sign of brittleness. Either restructure or delete the test, it should not be allowed into the test suite.
- The only exception is for tests that cover utility code with high complexity.
> [!note] Nota Bene
> Black-box testing is preferable for writing tests but white-box is great at analysing tests. A combination of both is best for test analysis.
> -> Use code coverage to see which branches are not exercised and then test them as if you know nothing of the code's structure.
## Citation
---
```
V. Khorikov, "The four pillars of a good unit test" in *Unit Testing: Principles, Practices, and Patterns*, 1st. M. Stephens, M. Michaels, S. Zaydel, A. Dragosavljevic, A. Calcara, T. Taylor, F. Buran, Eds. Shelter Island, NY, USA: Manning, 2020, pp. 67-91.
```