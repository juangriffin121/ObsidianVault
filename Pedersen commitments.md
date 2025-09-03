---
id: Pedersen commitments
aliases: []
tags:
  - Crypto
---

Using a finite group G of prime order q.
Get two generators on the group g and h such that log_g(h) is not known. (Potential backdoor? See [[Backdoor in elliptic curves]])
The commitment scheme has then parameters (G, q, g, h).
A commitment to the secret $s$ is then created as:
take a random t from the base field $t \leftarrow \mathbb Z_q$
$$c = C_{g,h}(s, t) = g^sh^t$$

When we want to open the commitment we just reveal s and t 


