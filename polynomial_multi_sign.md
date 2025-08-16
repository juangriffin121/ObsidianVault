---
id: polynomial_multi_sign
aliases:
  - Polynomial signatures
tags: []
---

A signature is a proof that its me and that that is the message i wrote. It needs to be able to be verified by anyone and it needs to depend on something only i know and the message i wrote, so that no one who isnt me can sign a message with my signature and messages are tied to the signatures so no one can change the message. 


# Polynomial signatures
N people, N needed for signature
if everyone knows a point, theres only one polynomial that goes through them all, and the y-intercept is unique for that polynomial and impossible to compute unless everyone shares their point maybe the y-intercept is then used as an RSA private key and a trusted party computes the public key. 
Issues with protocol as it stands:
- signers should make their points public
- trusted third party holding the public key.

## FROST

- each participant creates their own polynomial of t - 1 degree from t random values from Zq f_i(x) = \sum_j a_{ij} x^j
- each participant commits to their polynomial by computing C_{ij} = g^a_{ij} 
- each participant shares one point of his polynomial to each other participant, commonly his polynomial evaluated on the other's index, ie: P_i gives to P_k f_i(k)
- thus each participant gets one point for each of the N participants no one else knows. 
- since the polynomial is of degree t-1, any group of size t or bigger can compute the y-intercept of any participant (t points or more fix the polynomial everyone has one point of each participant's polynomial) all while a group of t-1 or less doesnt get any info
- thus a group of t or more can compute the y-intercept of all the polynomials thus they can compute the sum of all the polynomial's y-intercepts \sum_i a_{i0} = pk 

- Since the commitments C_{ij} are public everyone can have the commitment to the y-intercept of everyone C_{i0}
- g^k = PK = \prod_i C_{i0} is public

- the private and public keys can be used in a [[Schnorr_sig]] protocol
- in the actual implementation, each participant makes their own nonce r_i 
- commits to it with R_i = g^r_i
- they combine their R_i into an aggregate nonce as \prod_i R_i = g^\sum_i r_i
- they make the challenge C = H(R, X, M) to bind the sig to message and key [[interactive_zk_proofs]] [[non_interactive_zk_proofs]] for better idea of what the challenge does
- since everyone has the polynomials of all other participants evaluated in their own index f_k(i), they have the full polynomial in their index f(i) 
- by lagrange interpolation, f(0) = \sum_{i in group > t} \lambda_i f(i) = pk where \lambda_i = \prod_{j!=i} (0-j)/(i-j) (mod q)
- each participant computes the proof: S_i = r_i + C * lambda_i f(i)
- the S_i's are made public and are then summed to give: S = r + C * \sum_{i in group > t} \lambda_i f(i) = r + C * pk
- g^S = g^r * (g^pk)^C  = R * PK^C this can be checked publically

the relationship between the key and the message in the signature can be checked in the public world whereas it can only be created in the private world.

The public world g^x needs to keep some operations in the private world x so relationships can be checked while maintaining secrecy because going from public to private is hard.
