---
kind: article
links:
  - "[[???]]"
tags:
  - engineering_practices
publication:
  - paul-samuels.com
source: https://paul-samuels.com/blog/2025/02/23/project-scripts/
---
# Project Scripts

> [!abstract] Summary
> The author promotes the idea of creating a small cli tool in a projects' core language for managing any of its related administrative tasks. Doing this instead of one off bash scripts allows for a more maintainable project task system as well as opens the floor for wider team contributions and improved coding habits.
## Highlights
---
Try creating a ==**`cli` executable== in your project that exposes ==common project tasks== that are written in the ==project’s core language.**== This allows ==**better contribution== and ==less== single ==points of failure==** with pockets of knowledge in the team.

Over time projects accumulate helper scripts to perform various admin tasks. I’ve historically tried to avoid `bash` as much as possible for these scripts **because the projects I work on ==often have teams of people unfamiliar with `bash`== or its idiosyncrasies.**

The next logical step is to just use the main project’s language for building up tasks.

In taking this approach **I’ve ==removed myself== from being the ==single point of failure== on maintaining stuff.** This not only means that I don’t have to be on hand to debug things but also opens the door for easier contribution/reuse.

Using languages like `Swift`/`Kotlin` **encourages me to ==write more reusable code**== than if I was just slinging `bash` around. ... You have the **full ==power of available libraries== like type safe serialisation** with `Codable` or by pulling in something like `kotlinx.serialization`. ... ==**Debugging**== is a super power for these scripts ... the option to use a debugger and inspect all the things. ... I love **==types== and they are really handy for helping me ==write safe code.==**

I’m not 100% sold on the exact naming/layout but as a reference this is what I set up. We have a shim at the root of the project called `cli`, this file’s job is to essentially `cd` into the project that has the tasks and call `swift build` followed by running the built exectuable.

My other thinking is that if we come to ==**some standarisation== that in our projects you j==ust call `./cli` to be presented with all the various admin tasks**== then it’s just one thing to learn.
## Citation
---
```
"Project Scripts." paul-samuels.com. https://paul-samuels.com/blog/2025/02/23/project-scripts/
```
---
##### Completion Checklist
###### I. To Become Dark
- [ ] Complete the `Citation` section.
- [ ] Write the Full title in the heading.
- [ ] Complete the all metadata except `tags`.
- [ ] Read the article once in its entirety with focus, no music.
- [ ] Add tag `status/dark`.
###### II. From Dark to Dawn
- [ ] Read the article again, this time copy pasting interesting sections into the note under `Highlights`.
- [ ] **Bold** the portions of the `Highlights` you find most interesting.
- [ ] ==Highlight== the best parts of the bolded sections.
- [ ] Update status tag to `status/dawn`.
###### III. From Dawn to Day
- [ ] Write the article `Summary`.
- [ ] Fill in the context tags for the metadata.
- [ ] Update status tag to `status/day`.
- [ ] Remove this checklist.
