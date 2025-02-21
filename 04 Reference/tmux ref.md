**Prefix:** Caps + a

My caps lock now inputs as Control.

## Commands
### Session related

| Action                            | **Command**            |
| --------------------------------- | ---------------------- |
| Rename session                    | `prefix + $`           |
| New session                       | `tmux`                 |
| New session with name             | `tmux new -s <name>`   |
| Kill all session but the attached | `tmux kill-session -a` |
| Kill the current session          | `:kill-session`        |
### Window related

| Action                     | **Command**  |
| -------------------------- | ------------ |
| Create new window          | `prefix + c` |
| List all windows           | `prefix + w` |
| Rename window              | `prefix + ,` |
| Kill window                | `prefix + &` |
| Convert pane into a window | `prefix + !` |
| Previous window            | `prefix + p` |
| Next window                | `prefix + n` |
### Pane related

| Action                    | **Command**          |
| ------------------------- | -------------------- |
| Split window veritcally   | `prefix + %`         |
| Split window horizontally | `prefix + "`         |
| Switch pane to direction  | `prefix + arrow key` |
| Close current pane        | `prefix + x`         |
