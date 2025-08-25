---
id: Schnorr_sig
aliases: []
tags:
  - Crypto
---

on a [[cyclic groups|cyclic group]] with generator g and of order q (scalars are Z_q)
g^x is easy but the log is hard

public values are in uppercase, private values in lowercase except for g which is public but commonly written as lowercase

private key: x 
public key: X = g^x
message: M 

make random sample from Z_q call it r 
commit to r with R = g^r
This helps mask the private key (if not it could be leaked)

make a challenge:
C = H(R, X, M) this ensures the signature is bound to both the key and the message (See [[Fiat-Shamir transform]]  and [[non_interactive_zk_proofs]] for a deeper explanaiion of challenges)

compute the proof
S = r + C * x

the signature is then (R, S)

g^S = g^r * (g^x)^C  = R * X^C Notice how everything here is public

checking g^S == R*X^C verifies knowledge of the private key
