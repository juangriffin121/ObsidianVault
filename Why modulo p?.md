---
~
---
on a very simple encription scheme as the following formula:
$$k \cdot m = c$$
where k, m and c $\in F_p$ where p is a prime $2^{159} <p< 2^{160}$ 

gcd(c1, c2, . . . , cn) = gcd(k · m1, k · m2, . . . , k · mn) = k · gcd(m1, m2, . . . , mn) 

for a sufficiently large set of cs its very likely that gcd(m1, ...) is 1 or a small number since they are completely unrelated meaning gcd(cs) = k easy to compute. but using 
$$k \cdot m = c \mod{p}$$
doesnt have this problem.

 > This observation provides our first indication of how reduction modulo p has a wonderful “mixing” efefct that destroys properties such as divisibility. However, reduction is not by itself the ultimate solution. Consider the vulnerability of the cipher (1.9) to a chosen plaintext attack. As noted above, if Eve can get her hands on both a ciphertext c and its corresponding plaintext m, then she easily recovers the key by computin


# Why prime?
Prime numbers as modulo means gcd(n, p)=1 for all integers n, this means all the numbers up to p (the finite field Fp) have an inverse in the field. multiplication is closed under that ring 