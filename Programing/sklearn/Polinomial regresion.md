"What if your data is more complex than a straight line? Surprisingly, you can
use a linear model to fit nonlinear data. A simple way to do this is to add
powers of each feature as new features, then train a linear model on this
extended set of features. This technique is called polynomial regression."
```python
>>> from sklearn.preprocessing import PolynomialFeatures
>>> poly_features = PolynomialFeatures(degree=2, include_bias=False)
>>> X_poly = poly_features.fit_transform(X)
>>> X[0]
array([-0.75275929])
>>> X_poly[0]
array([-0.75275929, 0.56664654])
```
Note that when there are multiple features, polynomial regression is capable
of finding relationships between features, which is something a plain linear
regression model cannot do. This is made possible by the fact that
PolynomialFeatures also adds all combinations of features up to the given
degree. For example, if there were two features a and b, PolynomialFeatures
with degree=3 would not only add the features a , a , b , and b , but also the
combinations ab, a b, and ab .