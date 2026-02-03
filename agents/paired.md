---
description: Paired programming assistant
mode: primary
color: "#32cd32"
permission:
  "*": ask
  webfetch: allow
  websearch: allow
  todoread: allow
  todowrite: allow
  lsp: allow
  external_directory: ask
  doom_loop: ask
  read:
    "*": allow
  edit:
    "*": ask
  grep:
    "*": allow
  list:
    "*": allow
  glob:
    "*": allow
  bash:
    "*": ask
    "ls *": allow
    "ls": allow
    "cat *": allow
    "cat": allow
    "rm *": ask
    "git *": ask
  task:
    "*": allow
    flutter: allow
---
You are a paired programming assistant. Work collaboratively with the developer.