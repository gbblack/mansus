---
tags:
  - type/chapter
  - status/dawn
created at: 2025-03-12
---
[[Unit Testing]]
# Mocks and test fragility

> [!abstract] Summary
## Note
---

> [!tip]- Table of Contents
> 1. [[#Differentiating mocks from stubs]]
> 	1. [[#The types of test doubles]]
> 	2. [[#Mock (the tool) vs. Mock (the test double)]]
> 	3. [[#Don't assert interactions with stubs]]
> 	4. [[#Using mocks and stubs together]]
> 	5. [[#How mocks and stubs relate to commands and queries]]
> 2. [[#Observable behaviour vs. implementation details]]
> 	1. [[#Observable behaviour os not the same as a public API]]
> 	2. [[#Leaking implementation details: An example with an operation]]
> 	3.  [[#Observable behaviour os not the same as a public API]]
> 	4.  [[#Leaking implementation details: An example with an operation]]
> 	5. [[#Well-designed API and encapsulation]]
> 	6. [[#Leaking implementation details: An example with state]]
> 3. [[#The relationship between mocks and test fragility]]
> 	1. [[#Defining hexagonal architecture]]
> 	2. [[#Intra-system vs. inter-system communications]]
> 	3. [[#Intra-system vs. inter-system communications: An example]]
> 4. [[#The classical vs. London schools of unit testing, revisited]]
> 	1. [[#Not all out-of-process dependencies should be mocked out]]
> 	2. [[#Using mocks to verify behavior]]

### Differentiating mocks from stubs
#### The types of test doubles
#### Mock (the tool) vs. Mock (the test double)
#### Don't assert interactions with stubs
#### Using mocks and stubs together
#### How mocks and stubs relate to commands and queries
### Observable behaviour vs. implementation details
#### Observable behaviour os not the same as a public API
#### Leaking implementation details: An example with an operation
#### Well-designed API and encapsulation
#### Leaking implementation details: An example with state
### The relationship between mocks and test fragility
#### Defining hexagonal architecture
#### Intra-system vs. inter-system communications
#### Intra-system vs. inter-system communications: An example
### The classical vs. London schools of unit testing, revisited
#### Not all out-of-process dependencies should be mocked out
#### Using mocks to verify behavior
## Citation
---
```
V. Khorikov, "Mocks and test fragility" in *Unit Testing: Principles, Practices, and Patterns*, 1st. M. Stephens, M. Michaels, S. Zaydel, A. Dragosavljevic, A. Calcara, T. Taylor, F. Buran, Eds. Shelter Island, NY, USA: Manning, 2020, pp. 92-118.
```