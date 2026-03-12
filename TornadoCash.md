---
id: TornadoCash
aliases:
  - Tornado.Cash
tags: []
---

# Tornado.Cash
Tornado.Cash is a decentralized non-custodial protocol that allows private transactions in the crypto space.
To achieve this TC uses [[smart contracts]] **that accept token deposits from one address and enable their withdrawal from another**.
Those smart contracts work as pools that mix all deposited assets.

For traditional fixed amount pools:
* When a user puts funds into a pool he gets a private key to access it from any address and recover the funds. (whats a pool in this context? Its a smart contract that holds all deposits of a fixed denomination the pool of 1ETH)  
For Tornado Cash Nova, the new ETH pool with arbitrary amounts & shielded transfers:
* Doesnt use private key, funds are linked to a wallet address, users access their funds via their addresses, the strenfth of the protocol is linked to the number of users, i dont get how this one works? maybe later... 

## Contribution of zk-SNARK & hashing process
TC uses [[zk-SNARKs]] to verify and allow withdrawals.

> Let D = (dp, dv ) be the ZK-SNARK [Gro16] proving-verifying key pair for S created using some
trusted setup procedure. Let Prove(dp, T , k, r, l, A, f, t) → P be the proof constructor using dp and
Verify(dv , P, R, h, A, f, t) be the proof verifier.

T is merkel tree
l is leaf (position in the tree)
A is withdrawal address
f is fee
t is the relayer address which gets the fee (middle man)
R is the value of the merkel root used 
(A, f and t are context of the transaction)
The protocol uses two hashes, $H_1$ the [[pedersen hash]] and $H_2$ the [[MiMC hash]] with some tweaks to take in two inputs 

# Broad description of the protocol

- The smart contract stores the transaction information with a [[merkel tree]], it also stores the last 100 root values of the merkel tree in the history array. It also stores the values of nodes on the path from the last added leaf to the root that are necessary to compute the next root. 
- When depositing:
    - Generate 2 random numbers k and r and compute C = H1(k || r)
    - send ETH to the contract with C as the data. If the tree is not full, the contract accepts the transaction and adds C to the tree as a new non-zero leaf. (What if its full? new contract)
    - The path from the last added value and the latest root is recalculated.
    - The previous root is added to the history array.
- When withdrawing a coin (k, r):
    - Select a recipient address A and a fee f;
    - Select a root R among the stored ones in the contract and compute opening O(l) that ends with R. 
        I dont get it? select a root R from the history after the deposit, O(l) means the chain of sibling hashes that end in root, this is computed locally and used for the proof, thus not leaking the leaf. 
    - compute the hash of the nullifier h = H1(k)
    - compute the proof Prove(dp, T , k, r, l, O(l), A, f, t) → P (Notice O(l), i added it there but it has to be there bc of what i explained before) 
    - withdraw by:
        send an ETH transaction to the contract (empty?) with data (R, h, A, f, t, P) 
        do it via the relayer, still giving the same data
    - the contract verifies the proof with Verify(dv , P, R, h, A, f, t) (Note it doesnt have leaf position nor the secret values)
    - also verifies the coin was not withdrawn before, keeps track of the nullifier hashes.
    - if verif is succesful it sends the funds minus the fee to A
