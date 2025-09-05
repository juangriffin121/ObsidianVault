---
id: Virtual Memory
aliases: []
tags:
  - Programming
---

A program is assigned a specific region of [[memory|RAM]] by the [[kernel]] during its [[runtime]], however to make things simple and to avoid collisions and [[race conditions]] and [[undefined behavior]], the kernel creates for each program a virtual memory, whose size is bigger than the normal memory, where the [[Memory Address| address]] points to a virtual position which then gets mapped to the real memory (or the disk [[SwapMemory]] or some hardware [[device control|device]])

A program's virtual memory is usually separated into [[heap]], [[stack]] and [[code section]] (read only, contains constants, string literals, stuff initialized at compile time and that isn't changed)
