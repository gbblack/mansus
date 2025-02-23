---
tags:
  - type/chapter
  - status/dark
created at: 2025-02-22
---
[[Unit Testing]]
# The anatomy of a unit test

> [!abstract] Summary
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
#### Avoid multiple arrange, act, and assert sections
#### Avoid if statements in tests
#### How large should each section be?
##### The arrange section is the largest
##### Watch out for act sections that are larger than a single line
#### How many assertions should the assert section hold?
#### What about the teardown phase?
#### Differentiating the system under test
#### Dropping the arrange. act and assert comments from tests
### Exploring the xUnit testing framework
### Reusing test fixtures between tests
#### High coupling between tests is an anti-pattern
#### The use of constructors in tests diminishes test readability
#### A better way to reuse test fixtures
### Naming a unit test
#### Unit test naming guidelines
#### Example: Renaming a test toward the guidelines
### Refactoring to parametrized tests
#### Generating data for parametrized tests
### Using an assertion library to further improve test readability

## Citation
---
```
V. Khorikov, "The anatomy of a unit test" in *Unit Testing: Principles, Practices, and Patterns*, 1st. M. Stephens, M. Michaels, S. Zaydel, A. Dragosavljevic, A. Calcara, T. Taylor, F. Buran, Eds. Shelter Island, NY, USA: Manning, 2020, pp. 41-67.
```