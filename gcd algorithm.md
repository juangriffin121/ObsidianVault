
When i want to get $gcd(a, b)$

We know for any a and b:
$a = b \cdot q + r$

q is a // b and r is a%b

r is smaller than a

if d is a common divisor of a and b:

$a/d = b/d \cdot q + r/d$
we know both a/d and b/d $\in Z$ which means  $r/d \in Z$ else $r/d + b/d \cdot q$ wouldnt be integer.

This applies to any common divisor of a and b, which means it applies to the gcd, so if D is the gcd(a, b) then
$a/D = b/D \cdot q + r/D$
D is the greatest common divisor of a and b and it also divides r, it also has to be the greatest divisor of r that is common to b else if it had a bigger common divisor it would divide $a$ too. This means D is also the gcd(b, r), calculating that should be easier than the original problem because a>r.

putting it all together
$a = b \cdot q + r$
$gcd(a, b) = gcd(b,r)$

this gives us an efficient recursive algorithm to calculate the gcd. The algorithm stops when we get a remainder of 0:
$gcd(s,0) = s$ 
and s is then the solution and the gcd(a,b)

This is also called the Euclidian Algorithm

2024 = 748 · 2 + 528
748 = 528 · 1 + 220
528 = 220 · 2 + 88
220 = 88 · 2 + 44 
88 = 44 · 2 + 0 
gcd = 44

Another important theorem is that:
$gcd(a, b) = u a + v b$ with $\alpha \in Z$ and $\beta \in Z$
the gcd can always be writen as a linear combination of a and b with integer coefficients. theres multiple solutions in general, but they all belong to the same family:

if $(u_0, v_0)$ is a solution then all solutions have the form:
$$u = u_0 +\frac {b\cdot k}{gcd(a, b)}$$
$$v = v_0 +\frac {a\cdot k}{gcd(a, b)}$$




