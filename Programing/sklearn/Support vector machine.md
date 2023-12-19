```python
from sklearn.svm import SVC
svm_clf = SVC(random_state=42) 
svm_clf.fit(X_train[:2000], y_train[:2000])
```
For [[Multiclass]] classification problems you can use the SVM from sklearn and sklearn detects when you try to use a binary classifier for multiclass and chooses OvA or OvO accordingly.

### Linear SVMs
A support vector machine fits the two parallel lines separated by the maximal distance that splits the categories.

It can separate them with a hard margin, meaning you separate the two categories completely not allowing for any cases inside the separation "street", or soft margin, giving some tolerance, codified in the regularization hyperparameter c.

### Non linear SVMs
When the dataset is not linearly separable, we cant use linear SVMs.
One way to solve this is to not only use the features of the dataset but rather to use polinomials of those features.
If my dataset has two features x and y, i can create a bigger vector for the dataset, (x, y, x^2, y^2, xy) and use a linear svm to separate it. This can be done with the sklearn transformer PolynomialFeatures(degree=n)

```python
from sklearn.datasets import make_moons 
from sklearn.preprocessing import PolynomialFeatures 
X, y = make_moons(n_samples=100, noise=0.15, random_state=42) 
polynomial_svm_clf = make_pipeline(
								   PolynomialFeatures(degree=3), 
								   StandardScaler(), 
								   LinearSVC(C=10, max_iter=10_000, random_state=42) 
								   ) 
polynomial_svm_clf.fit(X, y)
```

The higher the degree of the polynomial, the more complex data it can separate, but the slower it is, the more combinatorics gets and the more chance of overfitting.

### Polynomial kernel

Fortunately, when using SVMs you can apply an almost miraculous mathematical technique called the kernel trick (which is explained later in this chapter). The kernel trick makes it possible to get the same result as if you had added many polynomial features, even with a very high degree, without actually having to add them. This means there’s no combinatorial explosion of the number of features.

### Gaussian RBF

Another technique to tackle nonlinear problems is to add features computed using a similarity function, which measures how much each instance resembles a particular landmark.

You may wonder how to select the landmarks. The simplest approach is to create a landmark at the location of each and every instance in the dataset. Doing that creates many dimensions and thus increases the chances that the transformed training set will be linearly separable. The downside is that a training set with m instances and n features gets transformed into a training set with m instances and m features (assuming you drop the original features). If your training set is very large, you end up with an equally large number of features.

### Gaussian RBF Kernel
Just like the polynomial features method, the similarity features method can be useful
with any machine learning algorithm, but it may be computationally expensive to
compute all the additional features (especially on large training sets). Once again the
kernel trick does its SVM magic, making it possible to obtain a similar result as if you had added many similarity features, but without actually doing so. Let’s try the SVC
class with the Gaussian RBF kernel:

```python
rbf_kernel_svm_clf = make_pipeline(
								   StandardScaler(), 
								   SVC(kernel="rbf", gamma=5, C=0.001)
								   ) 
rbf_kernel_svm_clf.fit(X, y)
```

### sklearn options for SVMs
The LinearSVC class is based on the liblinear library, which implements an opti‐
mized algorithm for linear SVMs. It does not support the kernel trick, but it scales
almost linearly with the number of training instances and the number of features.
Its training time complexity is roughly O(m × n). The algorithm takes longer if you
require very high precision. This is controlled by the tolerance hyperparameter ϵ
(called tol in Scikit-Learn). In most classification tasks, the default tolerance is fine.

The SVC class is based on the libsvm library, which implements an algorithm that
supports the kernel trick.The training time complexity is usually between O(m2 ×
n) and O(m3 × n). Unfortunately, this means that it gets dreadfully slow when the
number of training instances gets large (e.g., hundreds of thousands of instances), so
this algorithm is best for small or medium-sized nonlinear training sets. It scales well
with the number of features, especially with sparse features (i.e., when each instance
has few nonzero features). In this case, the algorithm scales roughly with the average
number of nonzero features per instance.

The SGDClassifier class also performs large margin classification by default, and its
hyperparameters–especially the regularization hyperparameters (alpha and penalty)
and the learning_rate–can be adjusted to produce similar results as the linear
SVMs. For training it uses stochastic gradient descent (see Chapter 4), which allows
incremental learning and uses little memory, so you can use it to train a model on
a large dataset that does not fit in RAM (i.e., for out-of-core learning). Moreover, it
scales very well, as its computational complexity is O(m × n). Table 5-1 compares
Scikit-Learn’s SVM classification classes.

## Under the hood
A linear SVM classifier predicts the class of a new instance x by first computing the decision function θ ⊺ x = θ0 x0 + ⋯ + θn xn , where x0 is the bias feature (always equal to 1). If the result is positive, then the predicted class ŷ is the positive class (1); otherwise it is the negative class (0). This is exactly like LogisticRegression
$$w^Tx + b = y$$
How about training? This requires finding the weights vector w and the bias term b that make the street, or margin, as wide as possible while limiting the number of margin violations. Let’s start with the width of the street: to make it larger, we need to make w smaller.

The bigger the vector your dotting x with, the smaller x needs to be to pass the threshold.
#### Hard margin
$$argmin_{w,b} (w^Tw) $$
constrained by:
$$y_{true}^i(w^Tx^i+b) >= 1$$ for i in range(len(instances))

#### Soft margin
To get the soft margin objective, we need to introduce a slack variable ζ (i) ≥ 0 for each instance:3 ζ (i) measures how much the i th instance is allowed to violate the margin. We now have two conflicting objectives: make the slack variables as small as possible to reduce the margin violations, and make ½ w⊺ w as small as possible to increase the margin. This is where the C hyperparameter comes in: it allows us to define the trade-off between these two objectives. This gives us the constrained optimization problem
$$argmin_{w,b} (\frac12w^Tw + C \sum_i \zeta^i) $$
constrained by:
$$y_{true}^i(w^Tx^i+b) >= 1 - \zeta^i$$ for i in range(len(instances))
when generating a loss for solving this optimization problem we can take the constrains and get the zetas
$$\zeta^i = max(0, 1 - y_{true}^i(w^Tx^i+b))$$
	And then putting them in the minimizing argument, getting the loss function, the hinge loss, the algorithm can use that or its square for the optimization.
### Kernel trick

