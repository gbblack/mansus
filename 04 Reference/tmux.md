# tmux
## My tmux

**Custom Prefix:** Caps Lock + a
**Mouse Mode:** enabled.
## Commands
### Core

| Scope   | Action                          | Command               |
| ------- | ------------------------------- | --------------------- |
| Session | Start a new session             | `tmux`                |
|         | Start a new session with a name | `tmux new -s name`    |
|         | List all sessions               | `tmux ls`             |
|         | Attach to last session          | `tmux attach`         |
|         | Attach to a specific session    | `tmux attach -t name` |
|         | Detach from the current session | `prefix d`            |
|         | Rename the current session      | `prefix $`            |
| Window  | Create a new window             | `prefix c`            |
|         | Rename current window           | `prefix ,`            |
|         | Move to next window             | `prefix n`            |
|         | Move to previous window         | `prefix p`            |
|         | List all windows                | `prefix w`            |
|         | Kill the current window         | `prefix &`            |
| Pane    | Split pane vertically ( \| )    | `prefix %`            |
|         | Split pane horizontally ( – )   | `prefix "`            |
|         | Navigate between panes          | `prefix arrow-key`    |
|         | Kill current pane               | `prefix x`            |
### Sessions

> [!NOTE]- All Session Commands
> | Action                                         | Command                                    |
> | ---------------------------------------------- | ------------------------------------------ |
> | Start a new session                            | `tmux`                                     |
> | Start a new session and name it                | `tmux new -s name`                         |
> | Start a new session and name it and the window | `tmux new -s s-name -n w-name`             |
> | Attach to the most recent session              | `tmux attach`                              |
> | Attach to a session by name                    | `tmux attach -t name`                      |
> | List all sessions                              | `tmux ls`                                  |
> | Kill a specific session                        | `tmux kill-session -t name`                |
> | Kill tmux and all sessions                     | `tmux kill-server`                         |
> | Exit tmux (kills current session)                    | `exit`                         |
> | Switch to a specific session (in tmux)         | `tmux switch -t name`                      |
> | Rename a session (outside tmux)                | `tmux rename-session -t old-name new-name` |
> | Detach from current session                    | `prefix d`                                 |
> | Rename current session                         | `prefix $`                                 |
> | Switch to next session                         | `prefix )`                                 |
> | Switch to previous session                     | `prefix (`                                 |
> | Show session list                              | `prefix s`                                 |
> | Choose client to detach                        | `prefix D`                                 |
> | Create new session                             | `prefix :new -s name`                      |
> | Switch to last session                         | `prefix L`                                 |
### Windows

- While in window list view (`prefix w`) to open and close the window drop downs you can use the `left` and `right` arrow keys; `left` to close and `right` to open.

> [!NOTE]- All Window Commands
> | Action                                    | Command                        |
> | ----------------------------------------- | ------------------------------ |
> | Create a new window                       | `prefix c`                     |
> | Rename current window                     | `prefix ,`                     |
> | Kill current window                       | `prefix &`                     |
> | List all windows                          | `prefix w`                     |
> | Find window by name                       | `prefix f`                     |
> | Next window                               | `prefix n`                     |
> | Previous window                           | `prefix p`                     |
> | Last active window                        | `prefix l`                     |
> | Go to window number ?                     | `prefix 0-9`                   |
> | Go to window by index                     | `prefix '`                     |
> | Move window to index                      | `prefix .`                     |
> | Enter command mode                        | `prefix :`                     |
> | Swap with previous window                 | `prefix {`                     |
> | Swap with next window                     | `prefix }`                     |
> | Create a new window                       | `tmux new-window`              |
> | Create a new window with a specified name | `tmux new-window - name`       |
> | Select window by index (0-9)              | `tmux select-wondow -t :0-9`   |
> | Select window by name                     | `tmux select-window -t :name`  |
> | Rename current window                     | `tmux rename-window "new name` |
> | Kill current window                       | `tmux kill-window`             |
> | List all windows in current session       | `tmux list-windows`            |
### Pane

> [!NOTE]- All Pane Commands
> | Action                               | Commands                    |
> | ------------------------------------ | --------------------------- |
> | Split pane vertically ( \| )         | `prefix %`                  |
> | Split pane horizontally (–)          | `prefix "`                  |
> | Convert pane to a window             | `prefix !`                  |
> | Kill the current pane                | `prefix x`                  |
> | Move between panes                   | `prefix arrow-key`          |
> | Go to next pane                      | `prefix o`                  |
> | Go to previous active pane           | `prefix ;`                  |
> | Show pane numbers                    | `prefix q`                  |
> | Toggle zoom on current pane          | `prefix z`                  |
> | Resize current pane                  | `prefix control+arrow-keys` |
> | Resize panes in steps of 5 cells     | `prefix Alt + arrow-keys`   |
> | Resize down by 10 cells              | `prefix :resize-pane -D 10` |
> | Swap current pane with prevous pane  | `prefix {`                  |
> | Swap current pane with the next pane | `prefix }`                  |
> | Cycle through preset layouts         | `prefix space`              |
> | Switch to even-horizontal layout     | `prefix Alt+1`              |
> | Switch to even-vertical layout       | `prefix Alt+2`              |
> | Switch to main-horizontal layout     | `prefix Alt+3`              |
> | Switch to main-vertical layout       | `prefix Alt+4`              |
> | Switch to tiled layout               | `prefix Alt+5`              |
## tmux taxonomy

**Server:** The background process that manages tmux sessions. A single server can manage multiple sessions.
**Session:** A collection of windows under a single name. Sessions can be detached (running in background) or attached (displayed in a terminal).
**Window**: Similar to a tab in a browser, each window occupies the entire terminal screen and can be divided into panes.
**Pane:** A rectangular division of a window running a single shell or program. Multiple panes can be displayed simultaneously.
**Client:** A terminal connected to a tmux session. Multiple clients can be attached to the same session simultaneously
**Prefix Key:** The key combination (usually Ctrl+b) that signals to tmux that the next keystroke is a tmux command.

## Customizing tmux config

To update `tmux` settings edit the `.tmux.conf` file found in the home directory.

| Setting           | Instruction                |
| ----------------- | -------------------------- |
| Save changes      | `tmux source ~/.tmux.conf` |
| Enable mouse mode | `set -g mouse on`          |
| Change prefix key | `set -g prefix <new key>`  |
## Sources

1. [Cheatsheet](https://tmuxai.dev/tmux-cheat-sheet/)
2. [Official Docs](https://github.com/tmux/tmux/wiki/Getting-Started)