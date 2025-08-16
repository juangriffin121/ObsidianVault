---
id: FermatLilTheorem
aliases:
  - Fermat's lil theorem
tags: []
---

# Fermat's lil theorem
> a^p = a (mod p)
> a^(p-1) = 1 (mod p)
if a is not divisible by p 

## Proof by Necklaces:
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
