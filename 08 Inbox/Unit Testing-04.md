---
tags:
  - type/chapter
  - status/dark
created at: 2025-02-26
---
[[Unit Testing]]
# The four pillars of a good unit test

> [!abstract] Summary
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
### The third and fourth pillars: Fast feedback and maintainability
### In search of an ideal test
#### Is it possible to create an ideal test?
#### Extreme case 1: End-to-end tests
#### Extreme case 2: Trivial tests
#### Extreme case 3: Brittle tests
#### In search for an ideal test: The results
### Exploring well-known test automation concepts
#### Breaking down the Test Pyramid
#### Choosing between black-box and white-box testing
## Citation
---
```
V. Khorikov, "The four pillars of a good unit test" in *Unit Testing: Principles, Practices, and Patterns*, 1st. M. Stephens, M. Michaels, S. Zaydel, A. Dragosavljevic, A. Calcara, T. Taylor, F. Buran, Eds. Shelter Island, NY, USA: Manning, 2020, pp. 67-91.
```
---
##### Completion Checklist
###### II. From Dark to Dawn
- [ ] Wait at least 5mins before beginning this section.
- [ ] In the `Note` section breakdown the chapter into its subheading: all the sections in bold.
- [ ] In bullet points, under each section and sub section heading summarise the major points of that section. In these bullet point make links to anything that could be referenced in a permanent note. This will take awhile.
- [ ] Update status tag to `status/dawn`.
###### III. From Dawn to Day
- [ ] Write the chapter `Summary`.
- [ ] Fill in the context tags for the metadata.
- [ ] Update status tag to `status/day`.
- [ ] Remove this checklist.