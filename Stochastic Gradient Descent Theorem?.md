
If i have a function F(x) whose gradient is hard to compute but i can get estimates  $\hat{\nabla F}$ that satisfy 
$$
E[\hat{\nabla F}] = \nabla F
$$
then i can use the update rule
$$
x \leftarrow x + \lambda \hat{\nabla F}
$$
and it ll converge to good local optimums of F

