---
tags:
  - type/article
  - status/dawn
  - tdd
  - software_design
publication:
  - WikiWikiWeb
source: https://wiki.c2.com/?DoTheSimplestThingThatCouldPossiblyWork
created: 2025-06-16
---
# Do The Simplest Thing That Could Possibly Work

> [!abstract] Summary
> A wiki thread discussing the idea "Do the simplest thing that could possibly work" from Extreme Programming (XP). The idea behind this advice is to reach for the simplest solution, i.e. the one you are most capable of, so that a working version is made available as soon as possible. From there refactoring is essential to bringing it to a quality standard. Attached to this idea is that the solution must be small and entirely understood or else it is not simple.
## Highlights
---
> "All that is complex is not useful. **==All that is useful is simple.==**" - Mikhail Kalashnikov

> First, ==**implement== a new capability in the ==simplest way you can think of==** that "could possibly work". Don't build a lot of amazing superstructure, don't do anything fancy, just put it in. Use an if statement, even.

> **==Second==**, and this is critical to the rule, ==**_refactor_ the system== to be the simplest possible code including all the features it now has.**

> **With ==each increment== of an Iterative Development, one should ==do the simplest thing that could possibly work**.== To do this, you have to know at least two ways to do the thing. That way, you can at least pick the simpler, if not the simplest.

> Know that the ways could possibly work. We do not mean will work, we mean could possibly work. **You need to be ==pretty sure it will work==, but you ==don't have to prove it.== Why? Because when you try to implement it, ==your implementation will tell you whether it _does_ work.**== Your tests will run, or they won't. It will feel good, or it won't.

> _We're not looking for the quickest way; we're looking for the simplest result. So, we first break the existing method into pieces. That leaves the existing test cases running. Then we modify (simply, now) one of the little methods to handle the next test case._

> What it does mean, however, is to ==**pick something you can do== and do quickly, so that you can get on with the other things you really need to do. ==Then do that thing professionally== and well, complete with all ==appropriate refactoring.**==

> **Remember that ==you can't make anything simple== (or, more accurately, simplest) ==until you fully understand it==**

> There are second order effects to asking yourself "What is the simplest thing that could possibly work?" - Kent Beck
> - You get done sooner
> - **==Your work is easier to communicate==**
> - Duplication is obvious, so the needs and means for refactoring are clearer
> - Tests are easier to write
> - The code is easier to performance tune _Or, at least, there will be more scope for tuning performance_
> - You feel less stress, which enhances all of the above

> **There is ==no reason why simplicity and quality are in conflict==: in my experience, ==they go hand in hand.== - Ron Jeffries**

> Remember, however, that "quality" is probably impossible to fully define. ... In the end, the "quality" of a software based system must be sufficient to meet the expectations of the users; the purpose of the system is really immaterial to the argument. - Wayne Mack

> Not so. Phil Crosby developed an entire school of thought on defining quality in non-fuzzy terms. Quality, very simply, is conformance to requirements. You can measure how far out of conformance your system is, so you know if it is a "good" system or not. The hard part is establishing the measurements, but domain-knowledgeable people can always find a way to do that.
---
```
"Do The Simplest Thing That Could Possibly Work." WikiWikiWeb. https://wiki.c2.com/?DoTheSimplestThingThatCouldPossiblyWork
```
