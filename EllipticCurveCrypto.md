---
id: EllipticCurveCrypto
aliases: []
tags:
  - Crypto
  - BasicMaths
---

Recall that a pillar in [[Cryptography]] are functions that are easy to compute but hard to reverse. 
ie given f(x) = y, its easy to compute y if you know x but hard to know x if you know y A particular kind of function that satisfy this criterion are the ones with a [[Discrete Log Problem]] y = f^n(x) = f(f(...f(x))) n times as in repeated application of f. if i give you y and x and you know the function f its still hard to find n and you'll have to try many times until you find it, for large values of n this is impractical while computing y is still easy knowing n, this creates **asymmetric information**   A common example for this kind of operations is exponentiation over [[finite fields]] 
An alternative is Point addition over elliptic curves.

### The curve 
The set of points (x, y) in F^2 (with F being the [[Field]] the curve is defined on) that satisfy:
$y^2 = x^3 + ax + b$
- The curve is defined by parameters a and b in F.
- The curve contains a point *O* or *I* depending on notation, we'll see what it is later
- This family of curves are symmetric around the y axis for a given x if y is solution -y is solution as-well
call it E(F) as the curve over F

### Point addition (or product)
If point P and Q are in E(F) then their sum (or product, depending on notation) is defined as follows:
- Draw the line that goes through P and Q
- Find the third intersecting point (The curve always has one) call it R
- Flip R's y coordinate, this is called -R we'll see why shortly, and thats the result of P+Q (or P*Q if defined as product)

### Scalar multiplication (or exponentiation)
Think of 2P as P+P
You cant really find the line that goes through P and P because no such thing, however, if we get P and Q and make Q get close to P, its easy to see that the line we end up on as a limit is the tangent of the curve at the point P 
For any integer number n, nG (or G^n) you can define it recursively as nG = G + (n-1) G  (or equivalent in prod not) 
Computing n from nG and knowing G is hard, whereas the formula nG is easy to compute.

### Formulas

#### Point Addition

Let $(P = (x_1, y_1)), (Q = (x_2, y_2))$ be points on the elliptic curve $(E: y^2 = x^3 + ax + b)$ over a finite field $(\mathbb{F}_p)$.

##### Case 1: $(P \neq Q), (x_1 \neq x_2)$
$$
\lambda = \frac{y_2 - y_1}{x_2 - x_1} \pmod{p}
$$
$$
x_3 = \lambda^2 - x_1 - x_2 \pmod{p}
$$
$$
y_3 = \lambda(x_1 - x_3) - y_1 \pmod{p}
$$
$$
P + Q = (x_3, y_3)
$$
---

##### Case 2: Point Doubling, $(P = Q), (y_1 \neq 0)$

$$
\lambda = \frac{3x_1^2 + a}{2y_1} \pmod{p}
$$

$$
x_3 = \lambda^2 - 2x_1 \pmod{p}
$$

$$
y_3 = \lambda(x_1 - x_3) - y_1 \pmod{p}
$$

$$
2P = (x_3, y_3)
$$

---

##### Case 3: Vertical Line, $(P = (x_1, y_1), Q = (x_1, -y_1))$
$$
P + Q = \mathcal{O}
$$
where $(\mathcal{O})$ is the **point at infinity**.

---

#### Scalar Multiplication

For an integer \(n\):
$$
nP = \underbrace{P + P + \dots + P}_{n \text{ times}}
$$
Efficiently computed via **double-and-add** (binary expansion of \(n\)).

---

#### The Point at Infinity $(\mathcal{O})$

The point at infinity acts as the **identity element** in elliptic curve addition:
$$
P + \mathcal{O} = \mathcal{O} + P = P
$$

Reason:
- Geometrically, the "line through $(\mathcal{O})$ and $(P)$" is interpreted as the vertical line through \(P\).  
- The third intersection point with the curve is the reflection of $(P)$, which is $(-P)$  
- Flipping its y-coordinate again gives back $(P)$.  

Thus $(\mathcal{O})$ behaves exactly like "0" in addition or "1" in multiplication — it is the neutral element of the group law.

### asdasdasd
Finding a point G and multiplying (or exponantiating) it to a number x is an operation with a hard logarithm, thus its used in a lot of [[Cryptography]] protocols [[ecdsa]] [[diffie_hellman]] [[shamir_secret_shareing]] and more.
The protocols pick particular choices for the curve's parameters, and the point G. There's also a number called n which is defined as 
$$
nG = \mathcal{O} 
$$

Its easy to see that on n+1 its gonna be G again, thus the whole operation repeats and its like mod n, this makes G create a [[cyclic groups|cyclic group]] of order n 


