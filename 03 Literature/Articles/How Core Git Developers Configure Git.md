---
tags:
  - lit-note
source: https://blog.gitbutler.com/how-git-core-devs-configure-git
links:
  - "[[git]]"
---
# How Core Git Developers Configure Git

## Full config

```toml
# clearly makes git better

[column]
	ui = auto
[branch]
    sort = -committerdate
[tag]
    sort = version:refname
[init]
    defaultBranch = main
[diff]
    algorithm = histogram
    colorMoved = plain
    mnemonicPrefix = true
    renames = true
[push]
    default = simple
    autoSetupRemote = true
    followTags = true
[fetch]
    prune = true
    pruneTags = true
    all = true

# why the hell not?

[help]
    autocorrect = prompt
[commit]
    verbose = true
[rerere]
    enabled = true
    autoupdate = true
[core]
    excludesfile = ~/.gitignore
[rebase]
    autoSquash = true
    autoStash = true
    updateRefs = true

# a matter of taste (uncomment if you dare)

[core]
    # fsmonitor = true
    # untrackedCache = true
[merge]
    # (just 'diff3' if git version < 2.3)
    # conflictstyle = zdiff3
[pull]
    # rebase = true
```

---
## Makes Git Better

- View branches in column format, affects listing commands like `status` or `tag`.
  `git config --global column.ui auto`
- List branches by commit date, not alphanumerically.
  `git config --global branch.sort -committerdate`
- List tags so version numbers are treated as a series of integers.
  `git config --global tag.sort version:refname`
- Make the default branch `main` when you `init` a repo.
  `git config --global init.defaultBranch main`
- Use the histogram algorithm to improve diffs.
  `git config --global diff.algorithm histogram`
- Diff uses a different color for code that has just been moved.
  `git config --global diff.colorMoved plain`
- Detects if a file has been renamed.
  `git config --global diff.renames true`
- Will prefix the diff header saying where it is coming from. `i/` (index), `w/` (working directory), `c/` (commit) so we can tell what direction the changes are coming from.
  `git config --global diff.mnemonicPrefix true`
- Automatically create upstream branch if there isn't one yet.
  `git config --global push.autoSetupRemote true`
- Automatically push any local tags to remote.
  `git config --global push.followTags true`
- Automatically prune local branches whose remote have been deleted.
  `git config --global fetch.prune true`
- Automatically prune local tags whose remote no longer exists.
  `git config --global fetch.pruneTags true`
- Grabs all remotes when you run `git fetch` instead of just the default.
  `git config --global fetch.all true`

## Why the Hell Not?

- Will guess what command you meant and run it if you make a typo.
  `git config --global help.autocorrect prompt`
- Git commit message prompt will include the diff.
  `git config --global commit.verbose true`
- When rebasing with conflicts remember previous solutions.
  `git config --global rerere.enabled true`
- Will reapply the remembered conflict resolution.
  `git config --global rerere.autoupdate true`
- Configure a global `.gitignore`for files you never want to track (`.DS_STORE`).
  `git config --global core.excludesfile ~/.gitignore`
- Automatically squashes commits during an interactive rebase.
  `git config --global rebase.autoSquash true`
- Automatically stash any uncommitted changes, reapplying them after the rebase.
  `git config --global rebase.autoStash true`
- Update local branch references to point to the rebased commit.
  `git config --global rebase.updateRefs true`

## A Matter of Taste

- Include the original code when hitting a merge conflict not just the two conflicting changes.
  `git config --global merge.conflictstyle zdiff3`
- Make rebase the default `git pull` strategy.
  `git config --global pull.rebase true`

> [!note]
> The options below are useful only for large codebases and don't really make sense to be the default for every repository.

- Make a filesystem monitor in each repo so git status doesn't have to check every file to find the changes.
  `git config --global core.fsmonitor true`
- Caches untracked files in the monitor so that git doesn't keep looking for them.
  `git config --global core.untrackedCache true`