### Sessions

- `$ tmux`
  - Start tmux.
- `$ tmux new`
  - Create a new tmux session.
- `$ tmux new-session`
  - Start a new session.
- `$ tmux new-session -A -s mysession`
  - Start or attach to an existing session named `mysession`.
- `$ tmux new -s mysession`
  - Start a new session named `mysession`.
- `$ tmux kill-ses -t mysession`
  - Kill or delete session `mysession`.
- `$ tmux kill-session -t mysession`
  - Kill or delete session `mysession`.
- `$ tmux kill-session -a`
  - Kill or delete all sessions but the current one.
- `$ tmux kill-session -a -t mysession`
  - Kill or delete all sessions but `mysession`.
- `Ctrl + b $`
  - Rename session.
- `Ctrl + b d`
  - Detach from session.
- `Ctrl + b s`
  - Show all sessions.
- `$ tmux a`
  - Attach to last session.
- `$ tmux a -t mysession`
  - Attach to session `mysession`.
- `Ctrl + b w`
  - Session and Window Preview.
- `Ctrl + b (`
  - Move to previous session.
- `Ctrl + b )`
  - Move to next session.

### Windows

- `$ tmux new -s mysession -n mywindow`
  - Start a new session with the name `mysession` and window `mywindow`.
- `Ctrl + b c`
  - Create window.
- `Ctrl + b ,`
  - Rename current window.
- `Ctrl + b &`
  - Close current window.
- `Ctrl + b w`
  - List windows.
- `Ctrl + b p`
  - Previous window.
- `Ctrl + b n`
  - Next window.
- `Ctrl + b 0 ... 9`
  - Switch/select window by number.
- `Ctrl + b l`
  - Toggle last active window.

### Panes

- `Ctrl + b ;`
  - Toggle last active pane.
- `Ctrl + b %`
  - Split the current pane with a vertical line to create a horizontal layout.
- `Ctrl + b "`
  - Split the current with a horizontal line to create a vertical layout.
- `Ctrl + b {`
  - Move the current pane left.
- `Ctrl + b }`
  - Move the current pane right.
- `Ctrl + b Ctrl + b`
  - Switch to pane to the direction.
- `Ctrl + b Spacebar`
  - Toggle between pane layouts.
- `Ctrl + b o`
  - Switch to next pane.
- `Ctrl + b q`
  - Show pane numbers.
- `Ctrl + b q 0 ... 9`
  - Switch/select pane by number.
- `Ctrl + b z`
  - Toggle pane zoom.
- `Ctrl + b !`
  - Convert pane into a window.
- `Ctrl + b +`
  - Resize current pane height.
- `Ctrl + b -`
  - Resize current pane width.
- `Ctrl + b x`
  - Close current pane.

### Copy Mode

- `Ctrl + b [`
  - Enter copy mode.
- `Ctrl + b PgUp`
  - Enter copy mode and scroll one page up.
- `q`
  - Quit mode.
- `g`
  - Go to top line.
- `G`
  - Go to bottom line.
- `Spacebar`
  - Start selection.
- `Enter`
  - Copy selection.
- `Ctrl + b ]`
  - Paste contents of buffer.

### Misc

- `Ctrl + b :`
  - Enter command mode.
- `$ tmux list-keys`
  - List key bindings (shortcuts).
- `$ tmux info`
  - Show every session, window, pane, etc...

