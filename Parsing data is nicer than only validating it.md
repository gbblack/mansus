---
tags:
  - lit-note
source: https://mtsknn.fi/blog/parse-dont-just-validate/
links:
  - "[[type-driven design]]"
---
# Parsing data is nicer than only validating it

> [!abstract] Summary
## Why

- Parsing is taking some input and converting it to s more precise output.
- Parsing can increase type safety by knowing the result after parsing.
- A validator can only know if it was valid or not.
- Data transformation is the distinct difference between parsing and validating.
- You are not mutating it you are modifying to a better (more precise) format and returning this new format.
- Deal with messy data once, transformation happens on one place.
- Write functions against the ideal data shape.
this one has lots of links to other articles that are useful
## How

> [!quote]
