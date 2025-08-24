Definition (informal):
A one-way function is something that’s:

Easy to compute in the forward direction,

Hard to invert without special knowledge.


Different cryptographic primitives exploit different candidate one-way functions. Each family has a distinct hardness assumption and distinct uses.


---

1. [[Cryptographic Hash Functions]] (Strictly One-Way)

Example algorithms: SHA-2, SHA-3, BLAKE3.

Forward direction: Input → fixed-length digest.

Inversion hardness: Given digest h, it’s computationally infeasible to find any input x s.t. hash(x) = h.

Extra properties:

Preimage resistance: can’t invert.

Second-preimage resistance: can’t find a different input with same hash.

Collision resistance: can’t find any two inputs that collide.


Uses:

Integrity ([[Digital signatures]] commit-and-reveal, checksums).

Password storage (salted hashes, KDFs).

Proof-of-work (Bitcoin mining relies on hash preimages).


Distinctive feature:

They’re always one-way. There’s no “backward” decryption.

They’re not keyed; output doesn’t hide structure (you can always recompute forward).

They **preserve algebraic relations**.


---

2. Discrete log operations ([[Finite Fields]] [[ModularArithmetic]],[[EllipticCurveCrypto]] )

Example problems:

Discrete Logarithm Problem (DLP): given g^x mod p, find x.

[[diffie_hellman]] Problem: given g^a, g^b, compute g^(ab).


Forward direction: x → g^x mod p. Fast via square-and-multiply.

Inversion hardness: Discrete log is believed hard for sufficiently large primes.

Inversion hardness: Elliptic Curve Discrete Logarithm Problem (ECDLP).

Uses:

Diffie–Hellman key exchange.

[[Digital signatures]] (DSA, [[Schnorr_sig]]).

Public-key encryption schemes (ElGamal).

ECDH key exchange.

[[ecdsa]]/ EdDSA digital signatures.

---

4. [[RSA]](Trapdoor One-Way Functions)

Mathematical basis: Multiplication of large primes is easy; factoring is hard.

Forward direction:

Public exponentiation: c = m^e mod n (fast).


Inversion hardness:

Without the private key (which requires factoring n into primes), finding m from c is infeasible.


**Trapdoor**:

Knowledge of factorization of n yields private exponent d, which allows inversion.


Uses:

Encryption (RSA-OAEP).

Digital signatures (RSA-PSS).

Key transport (historically, less now).


Distinctive feature:

RSA is a trapdoor permutation: invertible only if you hold the trapdoor (secret key).




---

5. Symmetric Encryption as Pseudorandom Permutations

Example algorithms: AES ([[symmetric_encryption]]) ChaCha20.

Forward direction: Enc_k(m).

Inversion hardness: Without k, ciphertext looks pseudorandom; with k, easy to invert.

Uses:

Bulk data encryption.

Message authentication codes (HMAC with hashes, CMAC with block ciphers).


Distinctive feature:

Keyed one-way function: not inherently one-way; depends entirely on key secrecy.

Without key, appears one-way; with key, it’s invertible.




---

6. Other Special One-Way Constructions

Lattice-based functions:

Based on Learning With Errors (LWE). Hardness: solving linear equations with small noise.

Candidates for post-quantum crypto.


Code-based functions:

McEliece cryptosystem: relies on decoding random linear codes being hard.


Hash-based signatures (Lamport, XMSS):

Derive one-time or few-time signature schemes purely from hashes.


Distinctive feature:

Security based on problems believed hard even for quantum computers (unlike factoring/DLP).