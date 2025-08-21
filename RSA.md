---
id: RSA
aliases:
  - RSA
tags: []
---

# RSA
Public-Private key encription algorithm.

We need an operation which is easy in one direction, but hard in another unless yoou have a **trapdoor** in which case its easy

like [[diffie_hellman]] we have modular exponantiation as an operation which is easy in one direction but hard in the other.


## Basic behavior
Alice wants to send Bob a message that nobody else can read and Bob can know nobody else but Alice can have sent it.
Alice and bob have both private and public "keys", numbers that can be used to encript and decript messages.
If Alice encripts the message with her private key everyone can use her public key too decript it and they'll know its her who wrote it, but they can decript it.
If Alice encripts the message with Bob's public key, Bob is the only one who can decript it but he wont know its Alice.
If Alice encripts the message with both her private key and Bob's public key she gets the desired behavior.


we want:
- d(e(x, kP), kp) = x for all x in some range
- hard to get kp or decripting wihtout it 
- d(e(x, kp), kP) = x for all x in some range, you can use both private and public keys to encript and use the other to decript


### Facts
- Computing prime factors of large numbers is computationally hard, but making a large number from primes is super easy, just choosing the primes and multiplicate, this creates an **asymmetry of knowledge**

- a^{phi(n)} = 1 mod(n) <=> a and n are coprimes

- Euler's totien func \phi(n) = Number of coprimes to n up to n 

- Computing totien func of large numbers is computationally hard

- phi(pq) = (p-1)(q-1) where p and q are primes

### Formula
e(x) = x^a (mod m) = y 
d(y) = y^b (mod m) = x

- x is the plaintext
- y is the ciphertext
- e is the encription function
- d is the decription function
- m is the modulus its calculated as:
    pq Where p and q are large prime numbers far apart
- a is chosen aribitrarily with the constraint that:
    a^phi(m) (mod m) = 1 and a < phi(m) generally short bit length
- b is chosen such that:
    ab = 1 mod(phi(m)) (b is the modular multiplicative inverse of b modulo phi(n))
e(x) = x^a (mod m)
d(y) = y^b (mod m)

(e, m) are given as public key
d is kept as private key, p,q and phi(n) must also be kept secret since they can be used to calculate b, they can be discarded once d is calculated

### Explanation

#### Facts
- Fermat's lil theorem a^p = a (mod p) if a is not divisible by p this also means a^(p-1) = 1 (mod p)
    Proof by Necklaces:
        a^p can be thought of as the number of distinct possible strings of length p composed of a different characters
        we can group the strings by which other strings, if you tie the strings begining to end, the string is compatible with.
        if a is 2 and p is 5:
            AAAAB, AAABA, AABAA, ABAAA, BAAAA,
            AAABB, AABBA, ABBAA, BBAAA, BAAAB,
            AABAB, ABABA, BABAA, ABAAB, BAABA,
            AABBB, ABBBA, BBBAA, BBAAB, BAABB,
            ABABB, BABBA, ABBAB, BBABA, BABAB,
            ABBBB, BBBBA, BBBAB, BBABB, BABBB,
            AAAAA,
            BBBBB.
            As we can see aside from the strings that are only made up of one character, all other strings form part of one of 5 groups that consist of 5 elements.
        This wouldnt be the case if a was divisible by p:
            This is because strings like:
                AABAAB have substrings that they are just an integer copy of.
                They are just part of a group like this:
                    AABAAB, BAABAA, ABAABA
                any further permutations would make identical strings.
                They have a repeating substring in this case AAB which fits into p, and only the permutations of this substring form the group.
                If S is built up of several copies of the string T, and T cannot itself be broken down further into repeating strings,
                then the number of friends of S (including S itself) is equal to the length of T.
        When a is not divisible by p, there are some strings composed of all the same character, there are a characters so a of this strings.
        The rest of the strings use at least two distinct symbols from the alphabet.
        If we can break up a given string S into repeating copies of some string T, the length of T must divide the length of S.
        But since the length of S is the prime p, the only possible length for T is also p.
        Therefore, the above rule tells us that S has exactly p friends (including S itself).
        The second category contains a^p − a strings, and they may be arranged into groups of p strings, one group for each necklace.
        Therefore, a^p − a must be divisible by p, as promised.
- Euler's theorem is a generalization of Fermat's little theorem: For any modulus n and any integer a coprime to n, one has:
    a^phi(n) 1 (mod n)

we want in the end:
(x^a)^b = x (mod m)
x^ab = x (mod m)
x^ab = x (mod pq)
ab = 1 (mod phi(pq))

ab - 1 = k(p-1)(q-1)
k natural number

if x = 0 (mod p)
    x^(ab) = 0 (mod p) 
else:
    x^ab = x^(ab - 1)x = x^(k(p-1)(q-1))x = (x^(p-1))^(k(q-1)) x = 1^(k(q-1)) x (mod p) = x (mod p) via FLT
    idem for q 
Thus x^ab = x (mod pq)
(if i divide by p and get rest x and i divide that rest x by q to get rest x again, dividing by pq gives me rest x)

Thus if ab are 1 (mod (p-1)(q-1)) x^ab = x (mod pq)
Thus encripting by doing y = x^a (mod pq) and then decripting by y^b (mod pq) works  


In order for alice to prove that she is the one who made the message she can
Hash the message, and encripts the hash with her private key, she then sends the message encripted with Bob's public key together with this encripted hash(Digital Signature), 
