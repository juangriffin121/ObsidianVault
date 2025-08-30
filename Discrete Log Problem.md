---
id: 1756511800-discret-log-problem
aliases:
  - Discret Log Problem
tags: []
---

# Discrete Log Problem
The discrete logarithm problem is the property of some [[Group|groups]] where if we call the group operation a product

$$
A*B =C
$$
then exponentiaton defined as
$$
A^n = A^{n-1} * A
$$

calculating the logarithm is **believed** computationally hard, meaning its an [[NP-complete]] problem, we say its believed to be hard because [[P vs NP]] is not solved. 


> [!info] 
> - both are special cases of the hidden subgroup problem for finite abelian groups,
> - both problems seem to be difficult (no efficient algorithms are known for non-quantum computers),
> - for both problems efficient algorithms on quantum computers are known,
> - algorithms from one problem are often adapted to the other, and
> - the difficulty of both problems has been used to construct various cryptographic systems.


