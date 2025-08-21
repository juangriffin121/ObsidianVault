---
id: ManInTheMiddle
aliases: []
tags: []
---

A weakness in [[diffie_hellman]] 
Alice wants to establlish a secure connection with bob, via a network they cant trust, they decide on the diffie_hellman system to create a secret only them know to use as a key for [[symmetric_encryption]] of their future messages.
However Malcom an admin of the network intercepts their messages
he controls what messages go in and out, so he can pretend to be bob to alice and pretend to be alice to bob
Alice sends her g^a, Malcom intercepts it and pretends to be Bob, he has his own secret m and sends to alice g^m, alice would then think the shared secret with Bob is g^am and encrypt her messages with it, but malcom can read every message alice sends, and he can encrypt any fake message he wants with it too.
Bob is fooled in the same way, having a secret g^bm which he thinks is with alice but its actually with Malcom.

diffie_hellman isnt capable of solving this issue on its own, however using signatures with public keys like with [[RSA]] can solve this issue, the **https**  protocol uses [Eliptic curve] diffie_hellman with RSA to create secure connections 
