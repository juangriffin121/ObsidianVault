---
id: crypto
aliases:
  - 🔑 Key Insights on FROST, Schnorr, and Fiat–Shamir
tags: []
---
# 🔑 Key Insights on FROST, Schnorr, and Fiat–Shamir

[[polynomial_multi_sign]] [[Schnorr_sig]] [[shamir_secret_shareing]] [[interactive_zk_proofs]] [[non_interactive_zk_proofs]]

## 1. Elliptic Curve Operations vs Exponentiation
- In multiplicative groups: \(g^x\).  
- In elliptic curves: \(x \cdot G\) (scalar multiplication).  
- Not literal multiplication, but analogous: \(g^x \leftrightarrow xG\).

---

## 2. Polynomial Commitments and Homomorphism
- Polynomial: \(f_i(X) = \sum_j a_{ij} X^j\).  
- Commitment: \(C_{ij} = g^{a_{ij}}\).  
- Property:
  \[
  g^{f_i(k)} = \prod_j C_{ij}^{k^j}.
  \]

---

## 3. Threshold Key Generation (FROST-style)
- Each participant:  
  - Chooses polynomial, distributes shares.  
  - Publishes coefficient commitments.  
- Private key share = sum of constant terms received.  
- Public key = product of constant coefficient commitments.

---

## 4. Schnorr Signatures
- Key: \(X = g^x\).  
- Signing:  
  1. Pick random \(r\), compute \(R=g^r\).  
  2. Compute \(c = H(R, X, m)\).  
  3. Response: \(z = r + cx \pmod q\).  
- Verification:  
  \[
  g^z \stackrel{?}{=} R \cdot X^c.
  \]

---

## 5. Why Randomization Matters
- Using only \(z = x\) or \(z = cx\) breaks security.  
- Random nonce \(r\) ensures freshness and prevents leakage of \(x\).

---

## 6. Interactive → Non-Interactive (Fiat–Shamir)
- Interactive: verifier issues random \(c\).  
- Non-interactive:  
  \[
  c = H(m, R, X, \dots)
  \]  
- Hash ensures unpredictability of challenge.

---

## 7. Why Include \(X\) in the Hash
- Without \(X\): rogue-key attack possible.  
- Including \(X\): binds signature to specific signer’s key.

---

## ✅ Summary
- **Elliptic curves:** \(xG\) is the analogue of \(g^x\).  
- **Homomorphism:** makes commitments & threshold schemes possible.  
- **Schnorr:** \((R, z)\) with challenge \(c\) is the signature.  
- **Fiat–Shamir:** replaces interactive challenge with a hash.  
- **Include \(X\):** prevents rogue-key forgeries.


8. Suggested Path

Interactive Schnorr / Sigma protocols → discrete-log proofs

Fiat–Shamir → non-interactive Schnorr / signatures

[[Pedersen commitments]] + linear/arithmetic proofs

Bulletproofs → inner-product proofs → range proofs

zk-SNARKs / R1CS → circuits for arbitrary computations

Multi-party and threshold ZKPs

Research papers / advanced constructions
