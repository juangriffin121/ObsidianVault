# Privacy in the EVM: Tornado Cash & Privacy Pools

## Glosary

**Merkle tree**: A data structure where adjacent pairs of items are hashed together and the results are subject to this same process, iterating this process of hashing adjacent pairs creates a tree structure that allows for efficient verification of the content.
![[Merkle tree.png]]
The diagram above shows an example of a Merkle tree with four leaves.

**Merkle path**: A list of all the values in the branch of a particular leaf in the tree all the way up to the root, together with all the sister nodes which are needed to compute the next node in the branch, this provides an efficient $O(log_2(n))$ proof that a value is in the tree, this can also be used inside zero knowledge proofs to prove that a value is in the tree without revealing the value or the position in the tree.   
![[Merkle path.png]]
In the example of the diagram above, the list: `((L5; L6); (N1,3; N1,4); (N2,2; N2,1), Root)` is the Merkle path for the leaf L6, the blue values are the nodes belonging to the branch and the green values are the sister nodes.

**zk-SNARK**: Short for zero knowledge succinct non-interactive arguments of knowledge, are cryptographic proofs that a user knows a secret value that satisfies some constraints without revealing any information beyond the fact being proved. They are succinct because the proofs are small in size and verification is computationally cheap, and non-interactive because they don't require interaction between prover and verifier.

**Circuit:** In the context of zk-SNARKs a circuit refers to a set of variables also called signals and constraints between them, these constraints are built from basic arithmetic operations (equality, addition and multiplication) and are used to model any computation done on some input signals. These circuits then allow a prover to generate a zk-SNARK of some secret values that satisfy some constraints with some public values.  

## Analysis:

### Anonymity model

#### Tornado Cash
This protocol accomplishes anonymity through a smart contract that the accepts ETH deposits from one address that can be withdrawn by another, without linking them in the process.
This makes the contract act as a pool that mixes all the deposited assets.
![[TornadoFlow.png]]
The contract stores the information about the funds in the pool in a Merkle tree, storing the last 100 values for the root and nodes needed to compute the next root. It also stores information of which funds are already withdrawn in a list of nullifiers, this last structure can be used to check upon withdrawal whether or not the funds to be withdrawn were so already, but its data can't be used to link to a particular deposit. 

Tornado Cash deposits are transactions to the smart contract with a fixed amount or denomination of ETH (there are multiple pools for different "coins" of 0.1 ETH, 1 ETH, 10 ETH, 100 ETH), if a user wants to deposit more funds they perform multiple transactions.

**How is this done?**

When depositing:
- The user sends the funds to deposit to the smart contract, generates two random values, the secret and the nullifier, and commits to them in the form of the Pedersen hash of their concatenation. 
- The contract then adds the commitment to its Merkle tree and updates the root.

When withdrawing:
- The user with a different address computes a zk-SNARK to prove that they know the pre-image of their leaf (and its presence in the tree) without revealing neither their secret nor the position of the leaf in the tree, the proof also includes as public inputs the user's new address and other context of the transaction. Alongside the proof, the user sends the hash of their nullifier to prove the funds were not withdrawn already.
- The contract verifies the proof and checks that the nullifier hasn't been spent and if successful transfers the funds to the new address and adds the nullifier hash to the list.

**What information can still leak?**
- Timing information: Since deposits and withdrawals are visible on-chain with a timestamp, timing patterns (both deposit-withdrawal intervals and time of day) can be detected to correlate them.
- Value correlation: Correlation between amounts deposited and withdrawn(a 7 x 10ETH deposit from one address and a 7 x 10ETH withdrawal to a single address can be linked), together with timing patterns can be used to deanonymize users, this and the previous item can be mitigated by good practices from the users.
- Anonymity set size: Tornado Cash uses fixed amount pools, this restricts the anonymity set to the size of the pool of the denomination deposited. This makes anonymity dependent on the number of users of the pool.
- Parameter reuse: Reusing address, gas prices, relayer fees, etc. leaks metadata that can reduce anonymity.
- Interactions outside Tornado: Patterns in the interactions of the deposit and withdrawal addresses outside of the protocol can be used to link them.

#### Privacy Pools 

Privacy Pools attempts to strike a balance between the anonymity provided by systems like Tornado Cash and regulatory compliance.
This protocol, like Tornado Cash, allows users to unlink their deposit address with their withdrawal address by leveraging zk-SNARKs and the Merkle tree structure to let users prove they have unspent funds in the pool without revealing which deposit those funds came from. But unlike Tornado Cash, in Privacy Pools a user also proves membership to a more restrictive set of deposits the **association set**, a subset of the set of all deposits that's specified by the user. This set and the proof of membership are public.

Its in this feature where the balance between compliance and anonimity of the protocol lies, it is often the case in practice that certain addresses are known to have illicit funds (most of the illicit funds that have been identified flowing into Tornado Cash have come from a DeFi protocol exploit, an event which is visible on the public blockchain), in Tornado Cash, if the user behind those funds follows best practices their withdrawal address is anonymized, this then stains honest users who just want to preserve their privacy, since, from a regulatory body's perspective, their withdrawal addresses are now just as likely to be associated with the illicit funds as those of the real culprit. This makes the protocol subject to sanctions and represents a risk for honest users. By allowing proofs of membership to association sets, Privacy Pools mitigates this problem, because honest users are incentivised to exclude known illicit deposits from their association set while keeping it as large as possible to maximize privacy, whereas the depositor of those funds can not exclude themselves from their association set and still withdraw their funds, this incentive structure can isolate well known bad actors, cleaning the image of honest user's addresses.

Another important difference between Privacy Pools and Tornado Cash is that the former supports arbitrary denominations, whereas the latter only supports coin deposits and withdrawals in the same denomination restricting the anonymity set. 

**How is this done?**

As previously explained, Privacy Pools like Tornado Cash, requires users to prove they have unspent funds in the pool without revealing where those funds came from. To this end the protocol also uses zk-SNARKs of the Merkle path and the pre-image of the leaf the user intends to withdraw aswell as a nullifier hash array to avoid repeated withdrawals.

The key difference between the protocols lies in the proofs of membership to association sets. To prove membership in the set, an associated Merkle tree can be constructed for the association set, the user provides the root of this tree as a public input and includes a separate zk-SNARK of a Merkle path from their leaf to that root. This proof could be stored on-chain or in another private repository where third parties concerned with compliance could verify them.

To support arbitrary denominations, Privacy Pools includes in the deposit commitment the amount being deposited, then upon withdrawal, this amount is compared to the public withdrawal amount within the zk-SNARK's circuit, the validity of the withdrawal amount is included in the proof, and remaining funds are saved to a new commitment.  

**How are association sets constructed?**

**Types**

- Inclusion sets: Association set consisting of **only** the deposits identified as low-risk
- Exclusion sets: Association set consisting of every deposit **but** the ones identified as high-risk.

Membership proof for both types of set are identical from a technical perspective since they both prove against the Merkle root of the association set. 

In practice, users don't manually curate their association set, instead, third parties called **association set providers(ASPs)** generate these sets and users choose an ASP according to the regulations they choose to prove compliance to and the ASP's own compliance requirements. 

**What information can still be leaked?**

Most of the issues in Tornado Cash are also relevant in Privacy Pools, however, since this protocol supports arbitrary denominations, the anonymity set is no longer tied to the denomination pool, but rather with the size of the association set used. Value correlation is still an issue because even if the funds are withdrawn in separate times and even by different addresses, the balance of the withdrawals could be correlated with the deposits potentially compromising anonymity.
### Security

#### Cryptographic assumptions

**Hash functions**

Tornado Cash uses the Pedersen hash for commitments and nullifiers and MiMC hash for Merkle tree construction. 

Privacy Pools uses the Poseidon hash for commitments, nullifiers and tree constructions. 

- **Pre-image resistance:** Given a hash `y`, finding `x` such that `Hash(x) = y` is computationally infeasible, This ensures commitments dont reveal their content, preventing attackers from withdrawing funds they didnt deposit. 
- **Second pre-image resistance:** Given `x1`, its computationally infeasible to find `x2` such that `Hash(x1) = Hash(x2)`, this ensures commitments are binding, so that the user cant find a second pair (secret, nullifier) that they could use to prove against the root (with a different nullifier hash) and thus double-spend their deposited funds.
- **Collision resistance:** Its computationally infeasible to get two inputs `x1` and `x2` such that `Hash(x1) = Hash(x2)`, this prevents users from simultaneously finding two different pairs (secret, nullifier) with the same commitment, creating ambiguous leaves. 

These properties (on the functions used to create them) also imply the integrity of Merkle trees and that Merkle paths are valid proofs of presence in the tree.

**zk-SNARKs**

Both Tornado Cash and Privacy Pools use Groth16 zk-SNARKs over either the BN254 or BLS12-381 elliptic curve groups, and so inherit the assumptions of this system. 
- **Soundness:** No adversary can produce a convincing proof for a false statement except with negligible probability.  
- **Zero-knowledge:** Proofs only reveal the statement being proven.

These properties rely on the assumption of **hardness of the discrete logarithm problem** in the elliptic curve groups used and the assumptions of the cryptographic hash functions used.
These cryptographic assumptions hold under the current state of computational power.  

- **Trusted setup ceremony:** Groth16 relies on a structured reference string (SRS) specific for every proving circuit used both during proving and verifying, this requires a random value used in the construction of the setup (the *toxic waste*) is not revealed, otherwise false proofs are possible. The ceremony of multi-party computation ensures that even a single honest participant results in an uncompromised setup.

#### Attack surfaces

- **Compromised zk-SNARK setup:** This is a critical vulnerability of systems dependent on SRS based zk-SNARKs, since a compromised setup would let attackers forge valid proofs and withdraw funds without deposits. To mitigate this issue the protocols perform multi-party computation ceremonies to decrease the risk of a compromised setup, for example the latest trusted setup ceremony ran by Tornado Cash enabled users to contribute directly from the web browser, resulting in a record-breaking number of 1114 participants. [[https://ceremony.tornado.cash/]].
- **Supply chain attacks:** In march 2024, Tornado Cash's source code was compromised with malicious Java Script code inserted within the user interface, concealing it in a valid governance proposal [[https://www.sciencedirect.com/science/article/pii/S0167404825003621#sec1]], obfuscated malicious code is becoming a recurring issue in open-source projects in general, such as the backdoor inserted in XZ utils in 2023, a common package in major Linux distribution, or malware published in public repositories like npm and PyPI disguised as useful packages.
- **Sanctions:** Particularly for Tornado Cash, its uncompromising stance on anonymity made it highly attractive to criminals including state-sponsored hacking groups, such as North Korea's Lazarus group was linked to the use of this protocol to launder funds coming from several large scale crypto hacks [[https://x.com/SecBlinken/status/1556677862345801728?s=20&t=6RbyLlka5BMgPmF0lvDDQg]]. These links to criminal activity make the protocol subject to sanctions from governments, which risks linking honest users' funds to such activity, such as the US sanctions in 2022 [[https://home.treasury.gov/news/press-releases/jy0916]].
- **Smart contract vulnerabilities:** Bugs in the smart contract can be exploited by attackers, for example a potentially exploitable bug was found by the Tornado Cash team themselves in the zk-SNARK implementation of the MiMC hash function, which would let attackers withdraw funds without a valid deposit. [[https://tornado-cash.medium.com/tornado-cash-got-hacked-by-us-b1e012a3c9a8]] 
- **Cryptographic vulnerabilities:** If any of the cryptographic primitives the protocols rely on was found to have weaknesses, they could be exploited by attackers.  

Most attack vulnerabilities are shared by both protocols since they rely on a very similar structure to validate withdrawals, Privacy Pools extra complexity adds to the attacks surface area but the prevalent links to criminal activity in Tornado Cash pose an inherent risk to users' funds. The vulnerability where these protocols differ the most is in their deanonymization vectors. 

#### Deanonymization vectors

**Common**
- **Timing:** Patterns in time intervals between deposits and withdrawals, as well as time of day information leak information about users that can link their addresses. [[https://link.springer.com/chapter/10.1007/978-981-16-9229-1_3]]
- **Value correlation:** Deposits and withdrawals of the same value can be correlated and link the addresses, this can still be an issue when withdrawn separately since the balance could still be correlated to the deposit (if I deposit 10 coins, and then withdraw 7.2859 and later 2.7141, those two withdrawals could be correlated based solely on the amounts). 
- **Outside interactions:** Interactions outside of the pools between the deposit and withdrawal addresses or the addresses and other parties reveals the users address clusters.
- **Cryptographic vulnerabilities:** If any of the cryptographic primitives the protocols rely on was found to have weaknesses, they could compromise the anonymity of the users.  
- **Parameter reuse:** Repeated use of the same parameters (Gas prices, particularly when a unique value is manually set, relayer and relayer fees) on different addresses.

Heuristics and machine learning methods (node embeddings) based on these deanonymization vectors can be successfully used to profile users, however most of these issues can be mitigated by users following best practices and achieve higher degrees of privacy [[arXiv:2005.14051v2]].

**Specific to Tornado Cash**
- **Denomination:** Since Tornado Cash is a coin based mixer, the anonymity set for a user who deposits in a particular denomination is restricted to the set of users of that denomination pool.   

**Specific to Privacy Pools**
- **Association set size:** Since Privacy Pools supports arbitrary denominations, the anonymity set of a user is not restricted by denomination, but rather, by the association set they prove membership to, which the user is free to choose, however the size of the association set is subject to a trade off between the privacy it provides and the security that the set doesn't include illicit deposits.
- **Malicious ASPs:** ASPs could provide different users different association sets, which could deanonymize them. This is why its recommended that the set root be published on-chain and that ASPs provide the whole set publicly.

### Compliance Stance

As discussed, compliance stance is the key difference between the two protocols, Tornado Cash provides undifferentiated anonymity, when following best practices, users' fund withdrawals can be coming from any deposit in the pool from the start of the contract up to the last deposit before their withdrawal, whereas in Privacy Pools, users prove membership to association sets, making the source of their withdrawal indistinguishable (when following best practices) from any deposit within their chosen association set. 

This difference in the protocols leads to different results for the users, in Tornado Cash users get a high degree of anonymity but it comes at the cost of their withdrawals being associated with deposits from illicit funds from dishonest users attracted by the anonimity provided. On the other hand, Privacy Pools' compromize of letting users choose their association sets aims to create a separating equilibrium between honest and dishonest users, this in turn disincentivices dishonest users from using the protocol. Privacy Pools aims to provide a neutral infrastructure for an ecosystem to grow around it where different ASPs could ensure compliance across various legal jurisdictions.  

### Composability

**Gas costs**

Tornado Cash lists this specs in this repo: [[https://github.com/tornadocash/tornado-core]]
- Deposit gas cost: 1088354 (43381 + 50859 * tree_depth)
- Withdraw gas cost: 301233

The Deposit gas cost scales linearly with the tree size since they require hashing up to the root and is the more expensive operation, withdrawal costs are constant on-chain since zk-SNARKs verification is O(1) in time but proving time also scales linearly with tree depth but this is done locally by the prover so it doesn't add to gas costs. 

Privacy Pools doesn't list these values, however, due to its increased complexity, its understood to be strictly more expensive than Tornado Cash. However the developers claimed:
> "There's almost no overhead added compared to the minimal design."

Referring to systems like Tornado Cash [[https://github.com/ameensol/privacy-pool]].

- Arbitrary denomination support and membership proof requires more public inputs slightly increasing proof verification gas costs [[https://orbiter-finance.medium.com/maximizing-efficiency-in-ethereum-zkps-a-look-at-groth16-and-fflonk-gas-costs-434c90a927a7]].
- If the association set roots are published on-chain as recommended by the protocol, the ASPs would have to pay gas costs to publish the roots with every update to the set, this cost could end up being paid by the user depending on the implementation of the ASP. 

#### Integration with dApps

Tornado Cash's contract architecture is designed to be minimal and self-contained, this however makes integration with other protocols or dApps challenging. In particular, the fixed denomination nature of the pools makes integrating composed fees, and dynamic value transactions hard to achieve. The protocol is mostly used as a one-off mixer, this isolation minimizes attack surface area.  

Privacy Pools on the other hand is better suited for integration with other DeFi protocols and dApps, its support for arbitrary denominations lends itself better to composition than Tornado Cash's system . Moreover, the compliance promise of this protocol presents an opportunity for downstream dApps (or auditors and banks) to require that incoming funds prove membership in a “clean” association set enabling on-chain, privacy-preserving compliance checks. 

ASPs can play multiple integration roles:
- **ASPs as compliance oracles:** A dApp can require proof against the set root of an ASP before accepting funds coming from the protocol, enabling automated, on-chain compliance gates.
- **ASPs as commercial services:** ASPs can charge users for inclusion in their set. Banks, exchanges or analytics firms may monetize such services as part of their compliance stack.
- **dApps publishing their own sets:** A DeFi protocol or exchange could publish an association set that excludes tainted deposits (or only includes safe deposits) so that only “clean” funds are accepted, effectively making ASP functionality part of the dApp’s risk management.

#### UX implications

**Tornado Cash** 

- Simple user flow (deposit, wait and withdraw) but rigid, fixed denominations and manual privacy hygiene make usage cumbersome and error-prone for non-experts.
- Lack of compliance means withdrawals can be rejected by dApps or exchanges.

The protocol has minimal interface complexity but best practices can be hard to master and make its use impractical, poor composability with DeFi UX patterns.

**Privacy Pools** 

- User's choice of association set adds a layer of complexity to withdrawal UX.
- Arbitrary denominations make deposits and withdrawals seamless for integrated DeFi use cases.
- Compliance visibility (via ASP roots) improves downstream usability, users know whether their funds will be accepted on regulated platforms.

Richer but more demanding UX, best practices for privacy still represent a challenge and the involvement of ASPs require trust or research by the user.

## Design

### Transfer/Swap system without compliance

**Transfer**

Alice wants to transfer a certain amount to Bob avoiding a public link between her address and Bob's, while still ensuring Bob knows it was her.
Any system that accomplishes this requires a secure communication channel between both parties, through this channel Bob could send Alice a commitment he created `Cb = H(kb, rb)` and Alice could deposit on a Tornado Cash style system (or even on Tornado Cash itself) the agreed amount with Bob's commitment, then Bob could withdraw the funds with his address. The anonymity provided by this style of mixer hides the link between the addresses.
Under this system Alice doesn't even need to know Bob's withdrawal address.

**Swap**

Alice wants to swap her token Ta for Tb, she finds Bob who wants the reverse, the transfer system proposed above represents a promising start for a private swap protocol.
If they could trust each other, they could use the secure communication channel to agree on a ratio and exchange commitments, (Alice sends Bob her commitment `Ca = H(ka, ra)` and Bob sends her `Cb = H(kb, rb)` ), and they would deposit their token with the other's commitments in the agreed ratio, and then they could withdraw with different addresses Tornado-style, with no public link between their accounts. Now we just need to create a trust-less system that accomplishes this behavior.

The smart contract could enforce the parties follow the "terms of their agreement" by only allowing Alice to withdraw Bob's deposit if she deposited too with Bob's commitment and the agreed amount. This has to be done carefully however, because this can reveal a link between their addresses which compromises anonymity. A verifiable link has to exist and it needs to be verifiable that the terms of the agreement are met, but the link shouldn't be public. Here's a potential solution:

Both deposits include both commitments (`Ca` and `Cb`), from now on called pre-commitments, and both agreed upon amounts of each token (stored as `Va` and `Vb` which contain information of amounts and tokens). These values however shouldn't be exposed or else linking the two deposit addresses is trivial, to hide them, the users can publish the hash of their concatenation, the public hashes should be different and finding the link between accounts just by seeing the two hashes shouldn't be feasible. A design that satisfies these requirements is the following:

Alice's hash = `H(Cb||Va||Ca||Vb)`
Bob's hash = `H(Ca||Vb||Cb||Va)`

The ordering of the values inside the hash is an arbitrary convention, it only matters that the hashes are different and that a convention is used, the one used here is centered around the withdrawal proof, when Bob wants to withdraw the deposit made by Alice, he proves he knows the pre-image of Cb which is the first "argument" to the public hash, and receives Va, then references his own deposit that he made with Alice's Ca and with Vb deposited.

Knowledge of the pre-image of these hashes can be used to prove both parties agreed to the same terms without leaking the link publicly. The values committed to have to be bound to the amounts actually deposited, this could be done by including the values actually deposited as public inputs to the proof verification, but that would defeat the purpose of hiding the link between addresses, a better solution would be for the contract to be able to check at deposit time that Alice deposited what she is agreeing to in the commitment, for this Alice would have to include in her leaf the raw value she deposited (this is public anyway) and the contract checks the deposited amount equals the declared amount. This doesn't bind the value inside the commitment to this declared value, but it can be used as a constraint within the proof's circuit that the declared value and the value inside the hash are the same. 

Including all this into a single leaf to prove its presence in the tree, can be done the following way:  
`leafAlice = H(Cb||Va||Ca||Vb) || Va` 
Where
- Cb is Bob's pre-commitment `H(kb||rb)`, Alice deposits with that so that Bob can withdraw her deposit.
- Va is Alice's declared value, an in-leaf raw value that is bound by the contract at time of deposit to the real amount deposited. Used to prove she followed her end of the agreement. The equality of the public Va and the Va inside the hash can be checked inside the zk-SNARK's proving circuit.

Symmetrically Bob's leaf is:
`leafBob = H(Ca||Vb||Cb||Va) || Vb` 

What Alice zk-SNARK proves:
- leafAlice and leafBob are in the tree (two Merkle paths to a valid root). Both deposits are performed.
- I know the pre-image `(ka, ra)` of the pre-commit Ca of the leaf that was deposited (by Bob) for me to withdraw.
- Both hashes bind the two deposits using the same two pre-commitments Ca and Cb. Both Bob and I agreed on this swap.
- The values in the hashes of both leaves match. Bob and I agreed on the same values.
- My raw declared value Va (enforced by the contract to match the deposited amount) is what I used inside the hash. I deposited what I agreed to. 
- The other party's raw declared value Vb is what they used to create their hash. Bob deposited what we agreed to. 

Alice's proof can be computed locally from the following inputs:
- Prover key.
- Valid root.
- Merkle path for leafAlice and for leafBob.
- Ca and Cb.
- Va and Vb.
- ka and ra.
- withdrawal address and other context.

And it can be verified with the following public inputs:
- Valid root
- Proof
- Nullifier hash 
- Withdrawal address and other context

If the proof is verified and the nullifier hash isn't spent, the funds are sent to Alice's withdrawal account.
The following is a diagram of the core flow of this design:
![[Private-swap design.png]]

The system as is ensures that if one party can withdraw then the other one can do so too, and the agreed upon amount, however, if any party doesn't abide by the agreement the other party wouldn't be able to create the proof either, but they don't have the secret of the deposit they made, so the funds would be locked in the contract, to avoid this the contract should have a timeout where if a certain threshold of time has passed and neither fund was withdrawn the funds go back to the deposit addresses. This threshold shouldn't be too short however, because an advantage of this system is that it allows both deposits and withdrawals to be done at different times, it just requires that the first withdrawal happens after both deposits were done, which allows users to time their transactions differently to keep the link anonymous.

There's still an inherent public link between the deposit addresses based on the public amounts, Alice and Bob probably used a public oracle to agree on the values that can be used to check the ratios and link the accounts, this issue is unavoidable in this system but it can be mitigated by the ecosystem (or a contract feature) encouraging (or enforcing) certain common amounts with ratios updating from the oracle so that Alice's deposit of her token can only be linked to multiple deposits of other tokens and not just Bob. Withdrawal anonymity can be further improved by allowing partial withdraws, like Privacy Pools does. 

 The system relies on a secure connection between users who want to swap their tokens, in practice a privacy-preserving decentralized pairing layer would find compatible users and establish a connection, this layer has its own design considerations to preserve user privacy, but for the purposes of this design we focused on how to provide an anonymous and trust-less swap system assuming an established secure connection between Alice and Bob and that the process of finding each other didn't leak their link.


### Transfer/Swap system with compliance

In a similar way to how Privacy Pools expands on Tornado Cash's core functionality to allow proofs of membership to more restrictive association sets to prove compliance, the transfer/swap systems designed above could be expanded upon by requiring that upon withdrawal users provide proofs of membership to an association set of their choosing.

**Implications for the system**
While the expansion for the transfer system is quite straight forward (Bob only withdraws the funds if he can prove Alice's deposit's membership to the association set, whatever Bob gave in return wasn't guaranteed by the protocol in the first place), the swap system requires some adjustments to maintain the fairness of the system with this expansion.

The funds Alice is withdrawing come from Bob's deposit. She could provide a proof that her own deposit belongs to some association set, but she cant be sure that Bob's address is clean and that she would be able to provide a similar proof for Bob's deposit, which might be asked of her by third parties to prove she's not associated with illicit funds. Here are two potential solutions to this problem where both parties can guarantee the other's compliance:

- **Binding the ASP in the commitment**: Although binding the root of the association set in the commitment is not possible, since the ASP only publishes roots constructed on current deposits, the commitment could include an id of the ASP, so when withdrawing, Bob would have to send as a public input the root of the association set and the id of the ASP that was committed to, the contract would check the validity of this root within the ASP (this requires the ASP publishes the roots on-chain), failure to be included would make Bob incapable of withdrawing, Bob would also have to generate a proof of Alice's membership to ensure either both are able to withdraw or neither is.  
- **Commitment lockers**: Alice could include in her deposit hash a secret locker value La `leafAlice = H(Cb||Va||Ca||Vb||La) || Va` which locks Bob from being able to provide proofs unless she gives him the locker if his deposit satisfies her compliance requirements, the issue with this system is that both Alice and Bob have lockers, and nothing prevents Bob from withholding his own locker from Alice, he doesn't have economic incentives to do so since he wont be able to recover the funds he deposited but its an issue nonetheless, this then becomes a fair-exchange problem, which, although ideal fair exchange with two parties has been proven to be impossible without trusted third parties, certain protocols either achieve weaker fairness properties or take advantage of stronger assumptions which could be sufficient for the protocol as it stands. [[https://doi.org/10.1007/0-387-23483-7_155]] [[https://doi.org/10.1007/978-3-031-47754-6_6]]   

Both parties would be required to publish (either on-chain or otherwise) both proofs of membership, one for the deposit they made and one for the deposit they are withdrawing (I'm not laundering money and I'm not helping someone else launder either), to maintain anonymity Bob's proof for his deposit's compliance should be different from Alice's proof of the compliance of the funds she's withdrawing. This is already guaranteed if the protocol uses Groth16 for the zk-SNARKs like the analyzed protocols do.

## Modeling

### Cost/feasibility of deanonymizing users in Tornado Cash style systems 

Deanonymizing users of these kind of systems means connecting a withdrawal address with a deposit address. The attacker can access any public data about the deposits, withdrawals and the transactions of the addresses outside of the mixer, anything they choose to use to infer the connection is stored in a vector associated with the deposit $\vec d$ or withdrawal $\vec w$. Based on the information they included, they would use a system to check withdrawals against deposits to choose candidates to deanonymize. 

**Estimation**

When presented with a pair (deposit, withdrawal) the attacker estimates the probability that they belong to the same user based on the data collected about them. This is represented as a function of the pair of data vectors.
$$P(connection) = f(\vec d, \vec w)$$ 
This function can be a simple heuristic of timing or value correlation (if the time interval between a deposit and withdrawal is less than 180 seconds the addresses belong to the same user) or it could involve much more complex analysis with machine learning algorithms and the transaction graphs of the addresses outside of the protocol. These more complex systems can yield better estimations but also involve higher costs. For the purposes of this model we can condense this into a complexity parameter $\gamma$ the attacker is free to choose. 

For this function to represent probabilities, the function requires the following properties:
$$1 \geq f(\vec d, \vec w) \geq 0$$
For every pair (deposit, withdrawal). 
$$\sum_{x \in D(w)} f(\vec x, \vec w) = 1$$
Where $D(w)$ is the set of all deposits done before the withdrawal analyzed. This is because the withdrawal has to be one linked to one of the deposits before it, and the reverse applies too, a deposit can only be linked to the withdrawals after it. These sets are large, but the attacker can reduce the size they check against through heuristics (most deposits are withdrawn within T time or less/checking links with larger time intervals would be too costly), strategies (Check addresses until the accumulated probability exceeds a threshold) and attacker knowledge (a known link between a deposit and a withdrawal allows the attacker to discard those as candidates).

**Targeting** 

The attacker would use their estimation system to find targets for deanonymization.
Two main attack methods are possible:
- **Withdrawal-centered:** Attacker focuses on a withdrawal and tries to find the deposit related to it. 
- **Deposit-centered:** Attacker focuses on a deposit and tries to find the withdrawal related to it (if its already withdrawn). 
In both methods, the attacker would have estimated the probabilities of a link between the query transaction and each element of a set of candidate transactions and based on the results he could choose two paths:
- **Discard the address:** If results indicate the real linked address isn't in the analyzed set or this set is too big and probability is too evenly spread across it for engagement to be profitable, the attacker would cut their loses and try to deanonymize a different address.
- **Engage the targets:** If results are more promising, they could choose a subset of the $k$ addresses most likely to be linked and engage them (inspect further, attempt extortion/seizure, etc). The attacker is successful and gets the rewards if the linked address is one of the $k$ targeted.

**Success model**
The probability of success $P_s$ for the attacker is the combined probability of a connection for the $k$ addresses in the subset to be the linked address, the attacker estimates these probabilities with the function $f(\vec d, \vec w)$, the complexity of this estimator is modeled here as the complexity parameter $\gamma$ which has an effect on its performance, captured by a dependence of the probability of success on $\gamma$, this probability also depends on the size of the subset $k$.   
For a withdrawal-centered method: 
$$P_S(\gamma, k) = \sum_{i=0}^k P(d_i | w) = \sum_{i = 0}^k f_{\gamma}(\vec d_i, \vec w)$$
For a deposit-centered method: 
$$P_S(\gamma, k) = \sum_{i=0}^k P(w_i | d) = \sum_{i = 0}^k f_{\gamma}(\vec d, \vec w_i)$$
**Reward model**

The reward model for a successful deanonymization would depend on the type of attacker.

- Government agents attempting to trace laundered funds would only get value out of linking deposit addresses known to posses illegal funds. The only value they would get in deanonymizing clean addresses is being able to discard the withdrawal addresses later. This means that their more likely avenue of attack would be a deposit-centered method, they'd start with a deposit from a known criminal address and use their estimator to check multiple withdrawals. The rewards $R$ they get will then depend on the nature and notoriety of the criminal, these rewards can be modeled as fixed values since the agents know this nature and that's why they focus on that address. Their reward model would be: $R = R(address)$   
- Blackmailers' rewards depend on both the economic capacity of the user they attempt to extort and their willingness to pay to remain anonymous. While some of this information could be known from the deposit/withdrawal address, it can still be difficult to predict and there's a baseline desire for privacy by the fact that they use the protocol which can be exploited. This type of attacker could use either withdrawal-centered or deposit-centered methods. Their reward model would be: $R = R_b + R(address) + \epsilon$ where $R_b$ is the baseline reward. The rewards for this type of attacker are a random variable in this model, $\epsilon$ accounts for the rewards associated with any information the attacker cant know or isn't willing to check about their target, whereas the explicit dependence on the address accounts for the value the attacker can expect from what they know about the address.  

While this difference is relevant to how the attackers would plan their attack, using a simple value/random variable $R$ for the reward of the attacker is sufficient for this model since it doesn't affect the attacker's choice of the main design parameters.  

**Cost model**

- Complexity cost $C_C(\gamma)$:  This cost, dependent on the complexity parameter, accounts for any costs that increase with the complexity of the estimator such as data collection (API fees, storage) and computational costs etc.  
- Engagement cost $C_E$:  This is the cost associated with engaging the targets.

The total cost of a single campaign with engagement is then:
$$C(\gamma, k) = k \cdot (C_C(\gamma) + C_E) $$
The cost of $n$ campaigns with $\alpha \cdot n$ engagements is:
$$C(\gamma, k, n, \alpha) = nk(C_C(\gamma) + \alpha C_E) $$
The expected cost of a campaign can then be calculated as such:  
$$C(\gamma, k, \alpha) = k(C_C(\gamma) + \alpha C_E) $$
Where $\alpha$ here is the probability of engagement, this value depends on the complexity of the model, the criterion for engagement (usually probability of success exceeding a threshold $\tau$) and the privacy hygiene of the users.

$$\alpha(\gamma, k, \tau) = P(Ps(\gamma, k) > \tau)$$

**Expected payoff and complexity trade-off**

The expected payoff $E(\Pi)$ for a campaign is then: 
$$E(\Pi) =  \alpha P_S(\gamma, k)R - k C(\gamma, k , \alpha)$$
$$ E(\Pi) = \alpha (\gamma, k, \tau) (P_S(\gamma, k)R - k C_E) - k C_C(\gamma) $$
The attacker is free to choose these parameters, complexity $\gamma$ (not choosing an actual numerical parameter, but rather he can choose the estimator's complexity), target set size $k$ and engagement threshold $\tau$, there's a trade-off to these values however, complexity can increase estimator performance but also costs, the target set size increases probability of success but also linearly scales costs and the engagement threshold lets the attacker cut their loses when probability of success is low, but produces a fraction of campaigns that will result in certain loses. The attacker would try to optimize the expected payoff with these parameters, if the optimal choice still yields a negative expected payoff attacks aren't profitable and so not feasible.  

While formulating a precise mathematical dependence on these parameters of the cost and probability of success would require assuming a particular form of the estimator and lose generality in the model, some general conclusions can be drawn from it and real world data can bring insight about these trade-offs in the current state of Tornado Cash's ecosystem and where attacks might be profitable: 
- Low complexity estimators like timing and gas-cost heuristics perform very well in deanonymizing users. [[https://doi.org/10.1007/978-981-16-9229-1_3]]
- Some complex estimators show improved performance, but data collection and computational costs increase rapidly with these methods [[https://doi.org/10.48550/arXiv.2005.14051]]. 
- Government agents would be more likely to use complex estimators and larger target sets ($k$) than ransom-seekers since they have specific targets, whereas blackmailers have a larger pool of potential targets and can settle for deanonymizing the easiest targets.  
- Following best practices can dissuade blackmailers from targeting the user, particularly since the average privacy-hygiene of Tornado Cash users is poor (you don't have to outrun the tiger, you just have to outrun the slowest person in your group).

Slight tweaks to the attack model presented here could improve an attacker's cost/benefit analysis, such as using a low cost estimator to filter out weak targets and refine the predictions on this smaller set with a better estimator, but the core trade-offs remain.
### Potential attacks on Privacy Pools

Privacy Pools use of association sets creates new avenues of attack beyond what it inherits from simpler mixers like Tornado Cash. 

**Association set overlaps**

If membership proofs are stored on the blockchain or or in another publicly accessible proof repository as recommended by the Privacy Pools team, the association sets restrict the users anonymity set. The association set for a withdrawal can be used in combination with those of other withdrawals to refine the attacker's estimation for all of them because of uniqueness constrains (one withdrawal comes from one deposit). 

For a set of deposits $D = \{d_1...d_N \}$, withdrawals $W = \{ w_1 ... w_M\}$ and association sets for those withdrawals $S = \{ S_1 ... S_M\}$ where the attacker wants to generate an assignment $a: W \rightarrow D$ that is injective (satisfying one withdrawal comes from one deposit). This sets would in general be a subset of all withdrawals and deposits since costs for this type of attack would be very high as will be discussed, in that case to account for the fact that a withdrawal could come from a deposit not in the set a value $d_n$ would be in $D$ to allow that mapping.

The attacker wouldn't just use a particular assignment, rather he would assign probabilities to possible assignments (only including assignments where the assigned deposit for every withdrawal is in their declared association set) based on the probability he assigns to each pair (deposit, withdrawal) in the assignment with his own estimator as discussed previously.
$$P(a) = \prod_{i=1}^{M} f(a(w_i), w_i)$$
These probabilities can be used to refine the estimation because baked into them is the information of association sets and what assignments they exclude (If $w_2$ proved membership to $S_5$ which doesn't include $d_3$ then $f(d_3, w_2)$ should be 0, discarding the possibility of $d_3$ being related to $w_2$ then increases the probability of the rest of the possible pairs).
$$f^*(d_j, w_i) = \sum_{a \in A| a(w_i) = d_j} P(a)$$
Where $A$ is the set of all possible assignments $A = \{a| a(w_i) \in S_i , \forall w_i \in W\}$. This set grows exponentially with the set of deposits and withdrawal and obtaining it from the association sets is a computationally hard problem, this process is infeasible for large deposit/withdrawal sets and doesn't provide much information with small sets. Certain algorithms can help refine estimations with less computational power (such as dynamic programming, graph neural networks, belief propagation etc) but these methods don't address another problem with this type of attack, the high cost of data collection. 

For every withdrawal the attacker wants to include, they would have to collect the entire association set they declared, since users have an incentive to prove membership to large sets, this can be too costly for the attacker.

**Malicious ASPs**

When an ASP doesn't publish their roots but rather gives individual users association sets/roots to prove against, it could provide different users different roots, which can give the ASP information to deanonymize users who prove membership to it.
The ASP has a set of trusted safe deposits, an honest ASP would use this as its association set, but this attacker would create subsets specifically designed to provide as much information as possible while maintaining ease of use. When users prove membership to these sets, the ASP can perform the same analysis explained above, use the constraints forced by the association sets of other withdrawals to update the probabilities, but now with carefully designed sets to make deanonymization easier and without the high data collection costs for retrieving the sets.          
Efficient methods and clever association set design can make this type of attack effective, however, not publishing the set roots can become a sign of malicious intent and thus users would be incentivised to prove membership to more transparent ASPs reducing the feasibility of these attacks.

This and the previous attack are forms of deanonymization attacks, as with Tornado Cash, two type of attackers could do them, blackmailers and government agents. Rewards for blackmailers are comparable but in general lower than for Tornado Cash, since Privacy Pools users are willing to sacrifice some anonymity for the ability to prove compliance, government agents on the other hand have much less incentives to deanonymize users because the separating equilibrium generated by the protocol's design makes it less attractive to criminals.

**Address tainting** 

An attacker could enter the pool with apparently clean addresses, becoming part of popular association sets. Later, the attacker can publicly link one or more of those addresses to illicit activity (for example, by signing a message that proves ownership of both the "clean" address and a known criminal address).  
This revelation marks any association set containing that address as tainted.  All users who relied on that set must now update their proofs or risk being classified as non-compliant. The attacker could use this to extort all users in those association sets, affected users face two costly choices:
- **Pay ransom $r$** demanded by the attacker to keep the address clean.  
- **Re-prove membership** in a new set that excludes the tainted address, incurring cost $C_U$ (gas fees or recomputation time).

A rational attacker will set the ransom such that $r < C_U$, making payment the cheaper option. Thus, the user’s cost per tainted address is $r$. The attacker can perform multiple rounds of extortion with all their tainted addresses, the payoff for an attacker performing $n$ rounds would be:
$$\Pi = \sum_{i=0}^{n} U_i \cdot r - n \cdot C_A$$
Where:
- $U_i$ is the number of users with addresses tainted by the i-th attacker address.
- $C_A$ is the cost for the attacker to set up a clean address to taint later (deposits, gas fees etc). 
The expected reward for a single round is:
$$E(\Pi) = U \cdot r - C_A$$
Where:
- $U$ is the expected number of users who would get their addresses tainted by an attacker address.

The attack is feasible for the attacker if:
$$U \cdot r > C_A$$
Remembering the constraint for the ransom:
$$ U \cdot C_U > U \cdot r > C_A$$
This forces the condition:
$$ U \cdot C_U > C_A$$
An attack is feasible if the combined cost for the expected number of users affected by one round exceeds the cost of setting up that round.

Possible mitigations include:
- **Incremental proofs:** Enable users to update proofs efficiently when addresses are tainted, reducing $C_U$.
- **Insurance pools:** Subsidize re-proofs or reimburse users for update costs.
- **Preemptive pruning:** ASPs can exclude risky addresses early through heuristics or more complex analysis increasing $C_A$ or the probability of success for the attacker in generating a clean address incurring the cost without reward.
