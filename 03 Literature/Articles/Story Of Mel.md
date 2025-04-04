---
tags:
  - type/article
  - status/day
  - mastery
  - story
publication:
  - WikiWikiWeb
source: https://wiki.c2.com/?StoryOfMel
created: 2025-04-04
---
# Story Of Mel

> [!abstract] Summary
> An article from 1983 about a "real programmer" Mel, who coded only in machine code. He was so good at it and so principled about controlling and understanding the machine that his programs, while unmanageable to anyone else, were optimised beyond what any general solution could do.
## Highlights
---
> A recent article devoted to the *macho* side of programming
> made the bald and unvarnished statement:
> Real Programmers write in FORTRAN.

> Real Programmers wrote in machine code.

> Machine Code.
> Raw, unadorned, inscrutable hexadecimal numbers.
> Directly.

> I feel duty-bound to describe,
> as best I can through the generation gap,
> how a Real Programmer wrote code.
> I'll call him Mel,
> because that was his name.

> I had been hired to write a FORTRAN compiler
> for this new marvel and Mel was my guide to its wonders.
> Mel didn't approve of compilers.

> "If a program can't rewrite its own code",
> he asked, "what good is it?"

> The new computer had a one-plus-one
> addressing scheme,
> in which each machine instruction,
> in addition to the operation code
> and the address of the needed operand,
> had a second address that indicated where, on the revolving drum,
> the next instruction was located.
> In modern parlance,
> every single instruction was followed by a GO TO!

> Mel loved the RPC-4000
> because he could optimize his code:
> that is, locate instructions on the drum
> so that just as one finished its job,
> the next would be just arriving at the "read head"
> and available for immediate execution.

> His code was not easy for someone else to modify.

> After he finished the blackjack program
> and got it to run
> ("Even the initializer is optimized", he said proudly),
> he got a Change Request from the sales department.
> The program used an elegant (optimized)r
> andom number generator
> to shuffle the "cards" and deal from the "deck",
> and some of the salesmen felt it was too fair,
> since sometimes the customers lost.
> They wanted Mel to modify the program
> so, at the setting of a sense switch on the console,
> they could change the odds and let the customer win.

> Mel balked.
> He felt this was patently dishonest,
> which it was,
> and that it impinged on his personal integrity as a programmer,
> which it did,
> so he refused to do it.
> The Head Salesman talked to Mel,
> as did the Big Boss and, at the boss's urging,
> a few Fellow Programmers.
> Mel finally gave in and wrote the code,
> but he got the test backwards,
> and, when the sense switch was turned on,
> the program would cheat, winning every time.
> Mel was delighted with this,
> claiming his subconscious was uncontrollably ethical,
> and adamantly refused to fix it.

>He had located the data he was working on
>near the top of memory ---
>the largest locations the instructions could address ---
>so, after the last datum was handled,
>incrementing the instruction address
>would make it overflow.
>The carry would add one to the
>operation code, changing it to the next one in the instruction set:
>a jump instruction.
>Sure enough, the next program instruction was
>in address location zero,
>and the program went happily on its way.

> Perhaps my greatest shock came
> when I found an innocent loop that had no test in it.
> No test.  *None*.
> It took me two weeks to figure it out.

> When I left the company,
> the blackjack program would still cheat
> if you turned on the right sense switch,
> and I think that's how it should be.
> I didn't feel comfortable
> hacking up the code of a RealProgrammer.
## Citation
---
```
"Story Of Mel." WikiWikiWeb. https://wiki.c2.com/?StoryOfMel
```

> [!note] Nota Bene
> The original post was to USENET by, Ed Nather, on May 21, 1983.
