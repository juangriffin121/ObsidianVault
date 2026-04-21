---
id: ModularArithmetic
aliases: []
tags: []
---


a = b mod c <=> a = q*c + b
a%c = b <=> a = b (mod c)

a = A (mod c) -> a = q * c + A
b = B (mod c) -> b = k * c + B
a * b = (q * c + A) * (k * c + B) = qkc^2 + qcB + kcA + AB
(a * b) % c = AB%c  

a%m%m = a%m
a%(bc) = a -> (a%b = a) and ()
a = q*(bc) + d q*bc id divisible by b and c

Thus 
>(a*b)%c = (a%c * b%c)%c

- Fermat's lil theorem a^p = a (mod p) if a is not divisible by p this also means a^(p-1) = 1 (mod p)

a^n+1 = x mod c -> a^n+1 = q * c + x
a^n+1 = a^n * a 
a^n = y mod c -> a^n = k * c + y

(k * c + y) * a = q * c + x
k * c * a + y * a = q * c + x 

(k * c * a + y * a)%c = (q * c + x)%c 

x = x (mod c)
(y * a)%c = x 
y = y (mod c)
y * a%c = x 

**Modular Exponentiation Algorithm** 
>a^(n+1) %c = (a^(n)%c * a%c)%c


# Modular inverse
 working mod $m$ a number $a$ has an inverse $b$
 meaning: $a \cdot b = 1$ mod $m$
  if and only if $gcd(a ,m) = 1$ meaning they are co-prime [[gcd algorithm]] 
This is because  if $a \cdot b = 1$ mod $m$ then $ab - 1 = cm$ and then $ab - cm = 1$
since the $gcd(a, m)$ divides both a and m then it should divide a linear combination of both so $gcd(a, m)$ divides $ab - cm$ but that is 1 meaning $gcd(a, m)$ divides 1, meaning it has to be 1 and a and m have to be coprime.


# The $\phi$ function

Any number $a$ inside the ring mod m {1, 2, ... m} that has an inverse, so $gcd(a, m) = 1$ is called a unit, its useful to know when working mod m, the number of units in the ring. The funcion that calculates it is called Euler's $\phi$ function. see  
#{0 ≤ a < m : gcd(a, m) = 1}
