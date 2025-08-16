---
id: non_interactive_zk_proofs
aliases: []
tags: []
---

The issue with [[interactive_zk_proofs]] is that they work for a single verifier, every new verifier that wants to checck the truth of the prover has to issue challenges themselves, because even if the interaction between prover and verifier_1 is public, he cant rule out that they are working together.

Thus every verifier needs to interact with prover which makes this not work as a public proof of knowledge of x, this can be solved with a system that is non interactive.

the formula 
z = r + cx 
with r being a random value chosen by prover to hide x and c being a random value chosen by verifier as a challenge to disallow prover to pick his values to solve the equation.
works well and a small tweak to it can make it non-interactive.

The trick is to introduce a challenge without needing the verifiers input.
a way this was done was by tying the value of c to R, in a way that can be verified and also makes the process of picking r to solve the problem infeasible.

c was needed because without it z = r + x, the prover could do R = g^z / X and the check would be solved without needing to know x, the introduction of a challenge that is produced by the antagonist eliminates the posibility of this choice, if c is chosen by the prover however this wouldnt provide security to the verifier because he is free to make it 1 or some other number he can solve easily, however, if he is not free to choose the value of c but rather his choice of r fixes c in a way which he cant predict and solve mathematically without bruteforcing the formula could work again without imput from the verifier.

z = r + c(R) x 
notice how c depends directly on R, which is tied to r so there's a dependance on r which makes picking r a challenge, and since it depends on the public R its verifiable.
to avoid mathematical trickery which could leave way to ways of picking r, c(R) should be an unpredictable and uninvertable function, exactly what hashes are made to be.

so c = H(R)
in general the heuristic is c = H(context)
where context includes all the data that should fix the proof uniquely and make it unforgeable.

What goes into the hash (the “context”)
- Essentials
    - Public statement: the value X=g^x (otherwise the challenge could be reused by other people eg:
        alice made a signature (R, z) where the c to create z doesnt depend on her Xa
        if Mallroy can pick a new key for herself she can pick Xm = (g^z/R)^(1/c)
        Then Alice’s signature verifies as if Mallory produced it, even though Mallory doesn’t know the discrete log.
        This works because c is the same for both XA and XM, since X isn’t included in the hash.

        This is called a rogue key attack 
    )
    - Commitment: the prover’s R=gr (ensures c is unpredictable when R is chosen).
    - Any public parameters: generator 
        g, group order 
        q, domain identifiers.
- Optional / good practice
    - Message being signed: if the proof is used as a signature (e.g. Schnorr signature), include the actual message m. This binds the proof to that message.
    - Protocol / domain tag: a string like "SchnorrZKProof-v1" to prevent cross-protocol reuse of the same hash challenge.
    - Participant identifiers: if many parties are involved, their IDs, keys, etc., to avoid replay attacks.
