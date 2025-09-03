---
id: PasswordStoring
aliases: []
tags:
  - Crypto
---

Tiers and issues:
- Plain text:
    anyone with access to the [[Database]] has all the accounts
- Encryption:
    slightly better but if anyone got the key, they have all the accounts
- Hashing:
    - only saving the passwords as their hashes [[Cryptographic Hash Functions]], 
    - computing the hash every time someone logs in and comparing, better, people with access to DB dont have the accounts
    - Weaknesses:
        Dictionary attacks: hashes of the most common passwords and run them through the DB, pregenerated pwd-hash DBs are called rainbow tables
- Hashing + Salting:
    To avoid the use of rainbow tables prepend a small random number to the pwd different for each user, save it in the db and also save the hash of the combinaton 
    Now rainbow tables wont find any info.
    - Weakness:
        The attacker has access to the salt, so they can salt for each user each of the most common pwds and hash that to find matches, slower than regular Dictionary attacks but feasible
- Slow Hashes:
    Make the life of the attacker in the last tier hard.

Probably best to just let google deal with it and let them sign with that
