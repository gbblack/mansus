---
tags:
  - tipsntricks
source: https://tylercipriani.com/blog/2022/11/19/git-notes-gits-coolest-most-unloved-feature/
links:
  - "[[git]]"
---
# Git Notes: git's coolest, most unloved­ feature

> [!abstract] Summary
> Git notes are and underutilised and underrealised feature of the git DVCS. They offer great potential for tracking the full history of a project (tickets, test reports, build systems, review history, …) rather than just the code. They are held back by their lack of UI equivalent in forges and clunky interface locally.
> 
## Why

- **Impart metadata**. With notes you can add metadata directly to a commit and the history. These could be ticket refs, test reports, build outputs, email threads. Anything that is not the code itself but would be useful to be associated with a commit.
- **Interaction history.** Notes can be setup to show who was requested for review, who reviewed, which tests run, etc. This can help collaboration by recording who has seen this code and what checks it has been placed under.
- **Offline.** Notes are part of git, they are baked into the history and can be accessed entirely offline and separate from any forges.
## Risks

- **Obscure.** Most forges (GitHub, GitLab, Bitbucket) do not have an equivalent UI for notes. They remain an obscure and unknown feature for most devs.
- **Limited.** Notes are designed to attach to commits. You can't attach notes to other git objects such as blobs or trees.
- **Clunky.** Notes are clunky to use, requiring familiarity with git's plumbing to really be beneficial.
## How

- **Built-in subcommand.** To add a note to the latest commit:
  `git notes add -m "note content"`
  The note is visible in the `git log` now, attached to the latest commit.