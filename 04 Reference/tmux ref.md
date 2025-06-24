**Prefix:** Caps + a

My caps lock now inputs as Control.
https://tmuxai.dev/tmux-cheat-sheet/

## Commands
### Session related

| Action                            | **Command**                                                                |
| --------------------------------- | -------------------------------------------------------------------------- |
| Rename session                    | `prefix + $`                                                               |
| New session                       | `tmux`                                                                     |
| New session with name             | `tmux new -s <name>`                                                       |
| Kill all session but the attached | `tmux kill-session -a`                                                     |
| Kill the current session          | `:kill-session`                                                            |
| Exit (detach) from a session      | `prefix + d`                                                               |
| Enter (attach) a names session    | `tmux attach -t <session name>`<br>`-t: target`<br>`attach aliases: a, at` |

### Window related

| Action                                                                                              | **Command**  |
| --------------------------------------------------------------------------------------------------- | ------------ |
| Create new window                                                                                   | `prefix + c` |
| List all windows (View)                                                                             | `prefix + w` |
| Rename window                                                                                       | `prefix + ,` |
| Kill window                                                                                         | `prefix + &` |
| Convert pane into a window                                                                          | `prefix + !` |
| Previous window                                                                                     | `prefix + p` |
| Next window                                                                                         | `prefix + n` |
| To open an close window lists while viewing use<br>the arrow keys, `left` to close `right` to open. |              |
### Pane related

| Action                    | **Command**          |
| ------------------------- | -------------------- |
| Split window veritcally   | `prefix + %`         |
| Split window horizontally | `prefix + "`         |
| Switch pane to direction  | `prefix + arrow key` |
| Close current pane        | `prefix + x`         |
