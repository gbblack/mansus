**Prefix:** Caps + a

My caps lock now input as Control.

## Commands
### Session related

|                                   | **Command**            |
| --------------------------------- | ---------------------- |
| Rename session                    | `prefix + $`           |
| New session                       | `tmux`                 |
| New session with name             | `tmux new -s <name>`   |
| Kill all session but the attached | `tmux kill-session -a` |
| Kill the current session          | `:kill-session`        |
### Window related

|                            | **Command**  |
| -------------------------- | ------------ |
| Create new window          | `prefix + c` |
| List all windows           | `prefix + w` |
| Rename window              | `prefix + ,` |
| Kill window                | `prefix + &` |
| Convert pane into a window | `prefix + !` |
| Previous window            | `prefix + p` |
| Next window                | `prefix + n` |
### Pane related

|                           | **Command**          |
| ------------------------- | -------------------- |
| Split window veritcally   | `prefix + %`         |
| Split window horizontally | `prefix + "`         |
| Switch pane to direction  | `prefix + arrow key` |
| Close current pane        | `prefix + x`         |
