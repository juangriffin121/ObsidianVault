---
id: Privacy Pools
aliases:
  - Privacy Pools
tags: []
---

# Privacy Pools

Privacy Pools attempts to strike a balance between transaction privacy (like with [[TornadoCash]]) and compliance (concerns about the legality of the transactions)
Uses zk proofs to ensure the user's association with a legitimate deposit without revealing transaction details.

**How does it go about it?**
- Association sets: Collections of legitimate deposits on the blockchain. Users aim to prove their membership in one of these sets showing that their funds arent associated with illicit activities.
- Membership proofs: Proofs of membership to one of these sets without revealing which. Based on zk-SNARKS.
- Proof storage: These proofs are stored publicly in the blockchain or in public repositories. Allowing regulators to verify compliance. 
- Privacy Thresholds: Privacy Pools often set specific thresholds for privacy. Users must meet these thresholds to provide proof of compliance. These thresholds help strike a balance between transparency and privacy, ensuring that both are maintained. 

In practice, users don’t manually choose their deposits for the sets. Instead, they subscribe to Association Set Providers (ASPs), **trusted third parties** who generate sets with specific characteristics. These ASPs can operate entirely on-chain without human intervention or off-chain, independently creating and publishing association sets. They analyze transactions using blockchain analytics for Anti-Money Laundering purposes and manage:
- Inclusion Proofs: These confirm transactions from “good” depositors. 
- Exclusion Proofs: These detect and exclude transactions from “bad” depositors.

In TornadoCash the user zk-proves he has a deposit in a leaf of the merkel tree through a merkel branch that leads to the root, in Privacy Pools, the user provides that same proof, with a proof of a branch of the tree representing their association set. Note that any subset of the pool has a tree with a root.  
**In between the user specifiying which deposit they withdraw, leading to no privacy, and providing no information beyond the precense in the tree and the coin not being spent, PP lets the user provide a set of possible origins for their funds**

> The public may not know Eve’s real-world
identity, but they have enough evidence to conclude that the
coins sent to the address that we are labeling “Eve” are stolen.
This is often the case in practice: most of the illicit funds that
have been identified flowing into TornadoCash have come
from a DeFi protocol exploit, an event which is visible on the
public blockchain.

There are two types of association:
- Inclusion Proofs: These confirm transactions from “good” depositors. 
- Exclusion Proofs: These detect and exclude transactions from “bad” depositors.
In technical terms the proofs are identical, since they involve proving belonging to the merkel root of an association set, one is the set of this good depositors, and the other is the set of all depositors except for this ones. 

**weaknesses**

malicious ASPs could give different users different association sets allowing them to deanonimize users, this can be avoided if the association set root is published on-chain where users can check other users with the same ASP for their root.