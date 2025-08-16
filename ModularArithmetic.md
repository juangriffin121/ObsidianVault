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
