---
id: mychshs
aliases:
  - mychshs
tags: []
---


# mychshs
My cheatsheet browser for command-line reference files

Usage: mychshs [OPTION] [cheatsheet]

Options:
  -l, --list          List all available cheatsheets (filenames without .md)
  -f, --fzf           Use fzf (or rofi) to interactively select a cheatsheet
  -h, --help          Display this help message

Without an option, the argument is interpreted as the name of the cheatsheet
to view (the script will look for a file named <cheatsheet>.md in $CHEATS_DIR).
EOF
