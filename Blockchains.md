---
id: Blockchains
aliases: []
tags: []
---

- every user has a pk PK pair
- Transaction: x gives amount to y signed by x
- Signature: [[Schnorr_sig]] or [[ecdsa]] or [[RSA]] By encrypting the hash of the message with private key verified by decrypting with public key
	- include in the hash an identifier of the transaction so as not to allow repeated use
- Transactions are broadcast to the net
- Group signed transactions by blocks
- Blocks are chained together to form a history of transactions, each block contains the hash of the previous block to ensure the order
- Miners prepare blocks to broadcast to the net, to do so they have to append a proof of work to the block, its a number that when appended to the block, it makes its hash have N* zeroes at the end, since its hard to find that, it makes creating fake forks of the blockchain to cheat infeasible. Another system that can acomplish this is proof of stake, where miners instead stakefunds and get to publish blocks in proportion to their stakes
