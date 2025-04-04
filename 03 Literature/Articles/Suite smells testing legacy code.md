---
tags:
  - type/article
  - testing
  - status/day
publication:
  - Bitfield Consulting
source: https://bitfieldconsulting.com/posts/testing-legacy-code
created: 2024-12-17
---
# Suite smells: testing legacy code

> [!abstract] Summary
> The article describes two process of testing legacy code:
> 1. Test as you go, in small increments.
> 2. As you work on old code break it down into smaller pieces and test those.
> 
>Doing this reliably over time will eventually create a well tested codebase.
## Highlights
---
> how to write **robust, reliable, and above all ==_correct_ programs==.**

> When you’re new to a project, or even when you’re not, it’s often worth **==stepping back== and taking a look at the ==test suite as a whole==.**

> One problem that you’re likely to see that affects a good **many projects, even today, is a ==complete lack of tests**==

> But **best practice is what a ==company _says_ it does.**== What _actually_ goes on is another matter.

> In the startup phase, **==speed== is more important than ==maintainability==.**

> the cost of **adding anything to an ==untested codebase becomes unmanageable==.**

> The answer is pretty simple: write some tests! Y**ou don’t need to get permission. Just go ahead and do it.** If you’re tasked with touching **some small piece of the code that ==isn’t tested, add a test==, then complete your task.**

> You ==**won’t== necessarily be ==praised== for doing this**, but it _will_ make your job easier,

> The prospect of writing a test for the _entire function_ fills you with horror and sadness (I’ve been there). Instead, **use the ==divide-and-conquer== technique.**

> See if a small block of lines around **where you’re working can be ==extracted== into its own==little function==.** If so, do this, **==add a little test== for the little function**, and make your change, leaving the rest of the code alone. ==**Repeat== this process every time** you touch some part of the system.

> any effort you put into test writing will automatically end up right where it’s needed: those parts of the system where you’re regularly making changes.

> But what if you can’t add a test for something because the **function is ==untestable?**==

> Here’s one ==**emergency== rescue procedure for untestable functions** that I’ve used successfully. **==Ignore the existing function== and write a ==test for the function you’d like to have.**== Then make that test pass by writing a new function from scratch. **Finally, ==delete the old function.==**

> We usually don’t need to make many changes to legacy systems, and when we do, a combination of testing and refactoring can make it safe. **The important thing to remember about ==legacy systems== is to ==avoid== writing more of them.**

> Some systems _have_ tests, but not enough of them, or they’re not very good.

> if someone is new **to writing tests, it will be a ==slow process at first==,** because they don’t know what to do. Once you have a little experience, testing becomes much easier and faster.

> agree with your team that **for a ==short trial== period, you’ll ==write tests for everything== you do.**.If anyone gets stuck on how to test something, the whole team will pitch in to help.

> If **the result is a ==correct program with robust tests==, then ==who cares what order==** those things are written in?
## Citatibon
---
```
"Suite smells: testing legacy code", Bitfield Consulting.
Available: https://bitfieldconsulting.com/posts/testing-legacy-code 
```
