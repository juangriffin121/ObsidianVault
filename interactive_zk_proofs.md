---
id: interactive_zk_proofs
aliases: []
tags: []
---

Also known as Interactive Schnorr proof

- I want to prove to you that i know a value x in Zq such that g^x = X in cycle group over Zq
- I dont want to make x public
- Since log_g(X) isnt feasible i can only know an x to the public X if i created X as g^x 

how do you know i know x and i didnt pick a random X?
i need to bind x to things in a way that someone can verify the binding without needing x itself
g^() provides useful properties in that regard too, g^(a+b) = g^a g^b and g^(ab) = (g^a)^b

random values also help
lets say i pick r from Zq, i can publish R = g^r and i can do arithmetic operations on x and r that can be verified by checking the relations in the g^() world
ie:
z = r + x -> g^z = g^r g^x = R X
if i publish (R,z)
this binds x to a random value, however you have no way to trust that R and r were chosen randomly, the proover can always choose 
R = g^z X^-1 
then the check g^z = R X would pass and the verifier would be decieved into believing th proover knew x

To fix this we can let the verifier and prover **interact** by the verifier issuing **challenges** to the prover in the form of multiple random values c for which the prover must be able to calculate 
z = r + c * x, publish (R, z), the verifier can check g^Z =? R X^c and be satisfied
Since the verifier is issuing the challenge he can rest assured that his challenge was random, and its unlikely that that the prover can solve for a response z without knowing x 

Why not just z = c * x?
then the verifier could ask for the challenge c = 1 then z = 1 * x and the prover must publish z which now is x his private key, no good. Even if prover doesnt allow c = 1, the verifier can ask multiple cs and recover x because they lie in a line. z1 - z2/ c1 - c2 (mod q)
"since z lives in the same space as x (Zq) but it has to be public, it needs randomness to hide x **blinding** the verifier"

thus z = r + c * x gives the verifier certainty of the prover's knowledge of x without sacrificing the secrecy of x.

