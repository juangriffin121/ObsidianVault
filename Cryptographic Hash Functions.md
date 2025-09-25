---
id: Cryptographic Hash Functions
aliases: []
tags:
  - Crypto
---
A [[Hash Function]] particularly suited for [[Cryptography]] protocols.
The criterion for this kind of functions are:
Given y = H(x)
- y is easy to compute from x
- x such that its hash is y (inversion hardness)
- its hard to find x2 with the same y as x1 (second reimage resistance)
- hard to find a pair of xs with (collision resistance)
