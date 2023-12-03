$$loss = MSE + r*Lasso + (1-r)Ridge$$
[[Ridge Regresion]]
[[Lasso Regression]]

>It is almost always preferable to have at least a little bit of regularization, so generally you should avoid plain linear regression. Ridge is a good default, but if you suspect that only a few features are useful, you should prefer lasso or elastic net because they tend to reduce the useless features’ weights down to zero, as discussed earlier. In general, elastic net is preferred over
