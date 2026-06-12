---
tags:
  - lit-note
source: https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/
links:
  - "[[best practices]]"
  - "[[type-driven design]]"
---
# Parse, don’t validate
**Parsing:** is a process that transforms input data into a more precise output. The transformation inherently validates the input while preserving the validation result in the more precise output.

The information is stored inside the type system. The unvalidated input state is one type which is parsed into a more explicit output type.
### How to apply this pattern?  -> Data types.

- Make data structures that make illegal states unrepresentable. Model these states with the most precise structures you can manage.
- Push the burden of proof up. i.e. get the data into its most precise shape ASAP (preferably at the boundary). You can use sum types to reflect control flow over the data structures.
- Write functions on the data you wish you had and then design data structures to bridge that gap.
- Data types inform code, not the other way around.
- Watch out for empty returns, make sure there isn't any information being lost in the function.
- You can parse in multiple passes. Avoid acting on data before it is fully parsed, you can use one partial parse to inform the next one. Sometimes parsing is context-sensitive.
- Avoid denormalised data , especially of it is mutable. Make a single source of truth. If denormalised data is required, hide it behind a module (encapsulate) responsible for keeping it in sync.
- Use abstract types to make validators "parse" data in situations where parsers are truly impractical.
---

> [!quote]
> the difference between validation and parsing lies almost entirely in how information is preserved

> [!quote]
> Consider: what is a parser? Really, a parser is just a function that consumes less-structured input and produces more-structured output.

> [!quote]
> parsers are an incredibly powerful tool: they allow discharging checks on input up-front, right on the boundary between a program and the outside world, and once those checks have been performed, they never need to be checked again!

> [!quote]
> In other words, a program that does not parse all of its input up front runs the risk of acting upon a valid portion of the input, discovering a different portion is invalid, and suddenly needing to roll back whatever modifications it already executed in order to maintain consistency.

> [!quote]
> The problem is that validation-based approaches make it extremely difficult or impossible to determine if everything was actually validated up front or if some of those so-called “impossible” cases might actually happen. The entire program must assume that raising an exception anywhere is not only possible, it’s regularly necessary.
> Parsing avoids this problem by stratifying the program into two phases—parsing and execution—where failure due to invalid input can only happen in the first phase.