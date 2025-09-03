---
id: End2EndEncryption
aliases: []
tags:
  - Crypto
---

Messages in the internet are sent through multiple servers, its not **direct** from alice to bob, simon in the server can read the message he has to relay
so to avoid him getting useful information, alice and bob generate a private symmetric encription key e with the [[diffie_hellman]] protocol with [[Digital signatures]] using [[RSA]] to avoid [[ManInTheMiddle|man in the middle]] attacks.
This is end to end because no server between alice and bob can read the messages
If alice or bob's devices are compromised, end to end encryption cant help them