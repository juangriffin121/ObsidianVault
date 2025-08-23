---
id: diffie_hellman
aliases: []
tags:
  - Crypto
---

Alice and Bob want to create a shared private key for [symmetric cryptography], to talk via encrypted messages only the two of them can decrypt.
Alice picks a secret number a, Bob does the same, b.
They publish the public counterparts, A = g^a, B = g^b with some [[cyclic groups|cyclic group]] with generator g which is agreed beforehand.
Now each can raise the others published number to their own secrets, Alice does B^a, Bob does A^b 
B^a = (g^b)^a = g^(ba)
A^b = (g^a)^b = g^(ab)
ab = ba 
Thus now they both know a number noone else can calculate from the public values and they can use it to encrypt and decrypt messages. 
