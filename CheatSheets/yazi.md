---
id: yazi
aliases:
  - Yazi File Manager - Keyboard Shortcuts Guide
tags: []
---

# Yazi File Manager - Keyboard Shortcuts Guide

Yazi is an efficient, user-friendly and customizable fast terminal file manager written in Rust based upon non-blocking async I/O. The name "Yazi" means "duck".

The following are the default keyboard shortcuts from `keymap.toml`.

## File Operations

| Task | Shortcut(s) | Notes |
|------|-------------|--------|
| Open Selected File(s) | `Enter` or `o` | |
| Open Selected File(s) Interactively | `Ctrl+Enter`, `O`, or `Shift+Enter` | Some terminals don't support Ctrl+Enter |
| Create File/Directory | `a` | End with a / (slash) for directories |
| Copy Selected File(s) | `y` | |
| Cut Selected File(s) | `x` | |
| Paste File(s) | `p` | |
| Paste File(s) (with overwrite) | `P` | Overwrites if files exist in destination |
| Cancel Copy/Cut File(s) | `X` or `Y` | |
| Delete Selected File(s) (trash) | `d` | |
| Delete Selected File(s) Permanently | `D` | |
| Rename Selected File(s) | `r` | |
| Copy File Path | `c c` | |
| Copy Directory Path | `c d` | |
| Copy Filename | `c f` | |
| Copy Filename Without Extension | `c n` | |
| Toggle Hidden File(s) Visibility | `.` | |
| Run a Shell Command | `;` | |
| Run a Shell Command (block until finish) | `:` | |

## Navigation

| Task | Shortcut(s) | Notes |
|------|-------------|--------|
| Back Parent Directory | `←`, `h`, or `H` | |
| Enter Directory Highlighted | `→` or `l` | |
| Forward Next Directory | `L` | |
| Move Cursor Up/Down | `↑`/`↓` or `k`/`j` | |
| Move Cursor Top | `g g` | |
| Move Cursor Bottom | `G` | |
| Page Up/Down | `Page Up`/`Page Down` or `Ctrl+b`/`Ctrl+f` | |
| Half Page Up/Down | `Shift+Page Up`/`Shift+Page Down` or `Ctrl+u`/`Ctrl+d` | |
| Move Up/Down 5 Units (preview) | `K`/`J` | |
| Change Directory Home | `g h` | Changes to `~/` |
| Change Directory Downloads | `g d` | Changes to `~/Downloads` |
| Change Directory Config | `g c` | Changes to `~/.config` |
| Change Directory Interactively | `g Space` | Changes to entered path |
| Change Directory with zoxide | `z` | |
| Change Directory/Reveal File with fzf | `Z` | |

## Find and Search

| Task | Shortcut(s) | Notes |
|------|-------------|--------|
| Find Next/Previous File | `/`/`?` | |
| Next/Previous Found Item | `n`/`N` | |
| Filter File(s) | `f` | |
| Search File(s) By Name with fd | `s` | |
| Search File(s) By Content with ripgrep | `S` | |
| Cancel Ongoing Search | `Ctrl+s` | |

## Selection

| Task | Shortcut(s) | Notes |
|------|-------------|--------|
| Toggle Selection | `Space` | Toggle highlighted file/directory |
| Visual Mode On/Off | `v`/`V` | Selection mode |
| Select All Files | `Ctrl+a` | |
| Select All Files Inverse | `Ctrl+r` | |
| Cancel Selection | `Esc`, `Ctrl+[`, or `Ctrl+c` | |
| Submit Selection | `Enter` | |

## Tab Management

| Task | Shortcut(s) | Notes |
|------|-------------|--------|
| New Tab | `t` | Uses current working directory |
| Switch Tab | `1` to `9` | Switch to specific tab |
| Switch Previous/Next Tab | `[`/`]` | |
| Swap with Previous/Next Tab | `{`/`}` | |
| Close Current Tab | `Ctrl+c` | Quits if only one tab |

## Sorting

| Task | Shortcut(s) | Notes |
|------|-------------|--------|
| Sort Naturally | `, n` | |
| Sort Naturally Reverse | `, N` | |
| Sort Alphabetically | `, A` | |
| Sort By Size | `, s` or `, S` | |
| Sort By Modified Time | `, m` | |
| Sort By Modified Time Reverse | `, M` | |
| Sort By Creation Time | `, c` | |
| Sort By Creation Time Reverse | `, C` | |
| Sort By Extension | `, e` | |
| Sort By Extension Reverse | `, E` | |
| Sort Randomly | `, r` | |

## Task Manager

| Task | Shortcut(s) | Notes |
|------|-------------|--------|
| Show Task Manager | `w` | Shows active tasks |
| Cancel Task | `x` | |
| Inspect Task | `Enter` | |
| Close Window | `w`, `Esc`, `Ctrl+[`, or `Ctrl+c` | |

## Misc

| Task | Shortcut(s) | Notes |
|------|-------------|--------|
| Help | `~` or `F1` | Display help screen |
| Quit | `q` | |
| Quit Without Writing CWD File | `Q` | |
| Suspend Process | `Ctrl+z` | |

## Configuration Files

| Path | Description |
|------|-------------|
| `~/.config/yazi` | User configuration directory |
| `~/.config/yazi/yazi.toml` | General configuration file |
| `~/.config/yazi/keymap.toml` | Keybindings configuration |
| `~/.config/yazi/theme.toml` | Color scheme configuration |
| `~/.config/yazi/flavors/` | Pre-made themes directory |
| `~/.config/yazi/plugins/` | Lua plugins directory |
