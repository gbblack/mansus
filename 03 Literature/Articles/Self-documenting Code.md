---
tags:
  - lit-note
source: https://lackofimagination.org/2024/10/self-documenting-code/
links:
  - "[[code practices]]"
---
# Self-documenting Code

> [!abstract] Summary
> Some rules of thumb to make your code more readbale and therefore "self-documenting"

- Use named constants -> instead of error code `500` make a constant `INTERNAL_SERVER_ERROR`.
- Move complex logic into its own functions -> instead of validating user input directly call a new `validateInput` function.
- Use [[short-circuit evaluation]] to remove any nested logic -> simplify conditional statements using logical operators (`validateInput(user) || throwErr(INTERNAL_SERVER_ERROR))`.
- Use type annotations on the parameters and returns of a function to enable static type checking at compile time -> use doc comments to allow for static typing guarantees in dynamic languages (`@param {User} user`).