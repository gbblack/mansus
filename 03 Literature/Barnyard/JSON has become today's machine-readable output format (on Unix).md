---
tags:
  - barnyard
source: https://utcc.utoronto.ca/~cks/space/blog/sysadmin/JSONModernMachineReadableFormat
links:
  - "[[JSON]]"
---
# JSON has become today's machine-readable output format (on Unix)

JSON's ubiquity makes it the good enough solution that we can all settle on. When handling machine readable output we should only use JSON, it works well enough already with existing tooling and anything new should only bother meeting this standard.
 
> [!quote]
> This experience has left me with the definite view that everything should have the option to output JSON for 'machine-readable' output, rather than some bespoke format. For new programs, I think that you should only bother producing JSON as your machine readable output format.

> [!quote]
> This isn't because JSON is the world's best format. Instead it's because JSON has a bunch of pragmatic virtues on a modern Unix system. In general, JSON provides a clear and basically unambiguous way to represent text data and much numeric data, even if it has relatively strange characters in it (ie, JSON has escaping rules that everyone knows and all tools can deal with); it's also generally extensible to add additional data without causing heartburn in tools that are dealing with older versions of a program's output. And on Unix there's an increasingly rich collection of tools to deal with and process JSON.

> [!quote]
> JSON has become a ["narrow waist"](https://www.oilshell.org/blog/2022/02/diagrams.html) for Unix programs talking to each other, a common coordination point that means people don't have to invent another format.
> 
> (JSON is also partially self-documenting; you can probably look at a program's JSON output and figure out what various parts of it mean and how it's structured.)
