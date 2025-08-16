---
id: shamir_secret_shareing
aliases: []
tags: []
---

- the secret sharer has a secret s he wants to share with a group in such a way that when a subgroup of it has more than a treshold t of participants they can reconstruct the secret, while if the subgroup doesnt meet the treshold they dont get any information.
- the secret sharer creates a polynomial of order t-1 using his secret as the y-intercept f(x) = \sum_j a_j x^j with a_0 = s and a_j j!=0 is a random Zq 
- he indexes each participant in the group as Pi 
- he evaluates his polynomial on the indexes of each participant and privately gives each his own. Pi gets f(i)
- a group of t or more can reconstruct the entire polynomial because t points fit one and only one t-1 polynomial, and thus they can get its y-intercept and get the secret.


