Average amount of information gain in bits when sampling from a distribution
Minimum amount of bits needed to encode the data.
$H(p) = E(\log(p)) = \sum \limits _x p(x)\log(\frac {1}{p(x)}) = -\sum p(x)\log(p(x))$

Receiving a bit of information reduces my uncertainty by a factor of two.

In an 8 sided dice i can tell you if the number was even or odd and you will be able to discard half of the possible results. And i can basically do the same again and just use the binary description of the number to convey the message. And the number of bits needed are the binary log of the number of possible outcomes. $log_2(8) = 3$  

If the probability distribution is not uniform, like if i have a coin that has 3/4 probability of heads and 1/4 of tails, we can imagine that there are 4 equally possible outcomes, 1 of which is tails and the other 3 are heads and therefore we can use the previous formula and calculate the number of bits with $log_2(4)$ if it landed tails.

If i have a uniform random byte of N bits it can represent a random process of $2^N$ equally possible outcomes, but it can also represent distributions over less possible outcomes but not uniformly distributed, a 2 bit byte can represent the previous weighted coin flip by making it so that only the (00) byte is tails and the rest are heads. 

If i receive a 

