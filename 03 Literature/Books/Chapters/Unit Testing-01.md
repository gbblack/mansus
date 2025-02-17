---
tags:
  - type/chapter
  - status/dawn
created at: 2025-02-10
---
[[Unit Testing]]
# 1. The goal of unit testing

> [!abstract] Summary
> An introduction to unit testing as tools to help create sustainable software. Includes brief overview of the why's and how's of the approache to unit testing taken throughout the book.
## Note
---

> [!tip]- Table of Contents
> 1. [[#The current state of unit testing]]
> 2. [[#The goal of unit testing]]
> 	1. [[#What makes a good or bad test]]
> 3. [[#Using coverage metrics to measure test suite quality]]
> 	1. [[#Understanding the code coverage metric]]
> 	2. [[#Understanding the branch coverage metric]]
> 	3. [[#Problems with coverage metrics]]
> 	4. [[#Aiming at a particular coverage number]]
> 4. [[#What makes a successful test suite?]]
> 	1. [[#It's integrated into the development cycle]]
> 	2. [[#It targets only the most important parts of your code base]]
> 	3. [[#It provides maximum value with minimum maintenance costs]]
> 5. [[#What you will learn in this book]]

### The current state of unit testing
- Currently there is consensus that we should be writing units tests, but not much understanding on what makes a good unit test.
- This lack of distinction can make testing a chore that can hinder a project.
### The goal of unit testing
- Unit testing leads to better code design as code that is hard to test is usually [[tightly coupled]] and therefore poorly designed.
- However test are only good at catching bad behaviour not guaranteeing good.
- In this way the goal of unit testing is aiding in sustainable software growth.
![[UnitTesting-001.png]]
- Good unit test can help avoid [[software entropy]], the observation that as a project grows its speed slows down.
- This entropy manifests in code where one change can lead to many bugs or fixing one bugs creates several others.
- Testing helps mitigate this by guarding against regressions, the occurrence of a feature not working as expected.
- This kind of testing requires significant upfront effort to make possible.
#### What makes a good or bad test
- Badly written tests do not aid in the goal of sustainable software, they in fact greatly hinder it.
![[UnitTesting-002.png]]
- Not all tests are created equal, more tests do not necessarily help the project as each test bears a cost:
	- Refactoring test along with underlying code.
	- Running the test on each code change.
	- False alarms.
	- Spending time on tests rather than the underlying code.
- With these costs in consideration only high quality tests should be written and maintained.
### Using coverage metrics to measure test suite quality
- Coverage metrics are used to try and measure test quality, this is usually done by looking at code coverage and/or branch coverage.
- A coverage metric shows how much underlying code the test suite executes on running, ranging from 0% to 100%.
- Higher coverage does not necessarily mean quality test, like the relationship between testing and design coverage is a good negative indicator but does not indicate anything positively.
- An example: if your coverage is 10% then that's good evidence you're not testing enough, however a coverage of 100% does not guarantee the quality of those tests only that the underlying code has ben executed. The tests being run could still be bad.
#### Understanding the code coverage metric
- Code coverage, aka test coverage shows the ratio of code lines executed by at least one test over the total number of lines in the codebase.
![[UnitTesting-003.png]]
- This metric mostly measure how compact the code base is and does not reflect the value of the test suite or the maintainability of the underlying code.
#### Understanding the branch coverage metric
- Branch coverage is more precise than code coverage because it focuses on the branches on structure such as if and switch statements.
![[UnitTesting-004.png]]
- It is still unreliable for guaranteeing quality since it cannot guarantee that all possible outcomes are being tested. It also does not account for any pathing done by external libraries.
![[UnitTesting-005.png]]
#### Problems with coverage metrics
- No coverage metric can take into account external libraries and as such cannot be relied upon for guaranteeing *quality* of a test suite.
- Nor can they give indication to exhaustiveness.
![[UnitTesting-006.png]]
#### Aiming at a particular coverage number
- Aiming for a particular number is not a good idea as what it is measuring is not related to the goal of maintainable software.
- They can still be used as an indicator for negative behaviour, such as not enough tests but does as a goal to aspire to.
### What makes a successful test suite?
- How to actually measure test quality? It must be done on an individual basis.
- There is no automated solution, evaluating each test needs to be done manually and hopefully gradually.
- Here are some criteria that make a test suite successful:
	- Integrated into development cycle
	- Targets only the important parts
	- Provides maximum value, with minimum maintenance
#### It's integrated into the development cycle
- Automated tests should be used constantly, ideally executed on every code change.
#### It targets only the most important parts of your code base
- Tests are valuable in how they themselves are structured but also in what underlying code they are targeting.
- Tests should be focused only on important, critical code with other code being checked briefly or indirectly.
- The important part of code is whatever contains the business logic, aka the domain model. Everything else falls into one of three categories:
	1. Infrastructure.
	2. External services.
	3. Code that glues everything together.
- Some of these parts still need thorough testing. They might also only be tested in integration tests which is fine for noncritical code.
- To follow this testing guideline the domain model needs to be isolated from the rest of the codebase.
#### It provides maximum value with minimum maintenance costs
- Maintainability of the test suite is paramount in making software sustainable.
- There are two skills needed for this:
	- Recognising a good test.
	- Writing a good test.
- To recognise a good test you only need a good frame of reference.
- However to write a good test you need to deeply understand the under test and therefore need to understand code design.
### What you will learn in this book
- This book will teach you:
	- A good frame of reference.
	- Existing unit testing techniques and practices.
	- Refactoring test suite and its production code.
	- Applying different styles of unit tests.
	- Integration tests to verify behaviour.
	- Identifying and avoiding [[anti-patterns]].
## Citation
---
```
V. Khorikov, "The goal of unit testing" in *Unit Testing: Principles, Practices, and Patterns*, 1st. M. Stephens, M. Michaels, S. Zaydel, A. Dragosavljevic, A. Calcara, T. Taylor, F. Buran, Eds. Shelter Island, NY, USA: Manning, 2020, pp. 25-41.
```