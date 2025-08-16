---
id: RSA2
aliases: []
tags: []
---

y = E(x, e, ) e is secret
x = D(y, d, )
d = f(e, )

Want: x = D((E(x, e, )), d, )

one way is:
y = x^e %m
x = y^d %m

Remember [ModularArithmetic](ModularArithmetic.md)


Want: x = D((E(x, e, )), d, )
x = (x^e %m)^d %m
x = ((x^e %m) (x^e %m) ... (x^e %m))%m
x = ((x^e) (x^e) %m... (x^e %m))%m
x = ((x^e) (x^e) ... (x^e) %m)%m
x = ((x^e)^d %m)%m
x = (x^e)^d %m%m
x = (x^e)^d %m

Remember [Fermat's lil theorem](FermatLilTheorem.md)
> a^p%p = a (p prime)
> a^(p - 1)%p = 1

notice that if m = pq (p, q primes)
x = (x^e)^d %pq
means
x = (x^e)^d %p
x = (x^e)^d %q



