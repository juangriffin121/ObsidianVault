Linear regression regularized(see [[Regularization]]) to keep small weights, idem [[Ridge Regresion]] with $l_1$ instead of $l_2$
$$ loss = MSE + \alpha\sum \limits_i |w_i|$$

> An important characteristic of lasso regression is that it tends to eliminate the weights of the least important features (i.e., set them to zero). In other words, lasso regression automatically performs feature 1 2 1 2 selection and outputs a sparse model with few nonzero feature weights.

