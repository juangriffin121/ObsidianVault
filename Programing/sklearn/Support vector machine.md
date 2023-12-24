```python
from sklearn.svm import SVC
svm_clf = SVC(random_state=42) 
svm_clf.fit(X_train[:2000], y_train[:2000])

svm_clf.decision_function(instance) #distance to the boundary  
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
$$y_{true}^i(w^Tx^i+b) >= 1$$
for i in range(len(instances))
Where ytrue_i is the category of the ith instance, its either -1 or 1

#### Soft margin
To get the soft margin objective, we need to introduce a slack variable ζ (i) ≥ 0 for each instance: ζ (i) measures how much the i th instance is allowed to violate the margin. We now have two conflicting objectives: make the slack variables as small as possible to reduce the margin violations, and make ½ w⊺ w as small as possible to increase the margin. This is where the C hyperparameter comes in: it allows us to define the trade-off between these two objectives. This gives us the constrained optimization problem
$$argmin_{w,b} (\frac12w^Tw + C \sum_i \zeta^i) $$
constrained by:
$$y_{true}^i(w^Tx^i+b) >= 1 - \zeta^i$$ for i in range(len(instances))
when generating a loss for solving this optimization problem we can take the constrains and get the zetas
$$\zeta^i = max(0, 1 - y_{true}^i(w^Tx^i+b))$$
	And then putting them in the minimizing argument, getting the loss function, the hinge loss, the algorithm can use that or its square for the optimization.
### Kernel trick

The dual problem:

make the w vector be a linear combination of the instances with positive components but signed according to category. This relies on the fact that w usually points from one category to the other.
$$w = \sum_i \alpha_i y_i^{true} \textbf x_i$$
Then the minimizing w condition turns into 
$$1/2w \cdot w = (\sum_i \alpha_i y_i^{true} \textbf x_i)\cdot(\sum_j \alpha_j y_j^{true} \textbf x_j) $$
$$1/2\sum_i \sum_j \alpha_i\alpha_j y_i^{true} y_j^{true} \textbf x_i \cdot \textbf x_j$$
for some reason they add the sum of the alphas
$$1/2\sum_i \sum_j \alpha_i\alpha_j y_i^{true} y_j^{true} \textbf x_i \cdot \textbf x_j + \sum_k \alpha_k$$
The constraints should become:
$$y^{true}_i((\sum_j \alpha_j y_j^{true} \textbf x_j)^T\textbf x_i+b) >= 1$$
$$y^{true}_i(\sum_j \alpha_j y_j^{true} \textbf x_j^T\textbf x_i+b) >= 1$$
$$y^{true}_i(\sum_j \alpha_j y_j^{true} \textbf x_j \cdot \textbf x_i+b) >= 1$$
but there's a constraint for positive alphas
$$\alpha_i>=0$$
and for some reason there's this one
$$\sum_j \alpha_j y_j^{true} = 0$$

believing that, we can get that the optimal w vector is easily obtainable with the alpha values:
$$\hat w = \sum_i \alpha_i y_i^{true} \textbf x_i$$
the bias term is just the average of the difference between the linear w on x and the outcome, so as to center the decision function
$$\hat b = 1/m \sum_j^m (y_j - \hat w^T \textbf x_j)$$
Then the prediction given a new data instance is:
$$y^{pred}_{new} = w^T \textbf x_{new} + b$$
$$y^{pred}_{new} = (\sum_i \alpha_i y_i^{true} \textbf x_i)^T \textbf x_{new} + b$$
$$y^{pred}_{new} = \sum_i \alpha_i y_i^{true} \textbf x_i^T \textbf x_{new} + b$$
$$y^{pred}_{new} = \sum_i \alpha_i y_i^{true} \textbf x_i \cdot \textbf x_{new} + b$$
Using the solution to the dual problem, we can see that both the optimization process and the prediction with new instances only depend on the dot products between the inputs and not the vectors themselves.

If we want to take non linearity into account by expanding the features of the dataset to include polynomial terms or something else, we would have $\phi(\textbf x_i)$ instead of $x_i$
and the dual problem would be formulated in terms of the dot products of this $\phi(\textbf x_i)$ vectors, however, in the case of polynomial features and other expansions we dont really need to calculate those features because the dot product $\phi(\textbf x_i) \cdot \phi(\textbf x_j)$ can be left as a function $K(\textbf x_i,\textbf x_j)$, this is called the kernel and simplifies a lot of those calculations.

so when making the prediction instead of writing:
$$y^{pred}_{new} = \sum_i \alpha_i y_i^{true} \phi(\textbf x_i) \cdot \phi(\textbf x_{new}) + b$$we write:
$$y^{pred}_{new} = \sum_i \alpha_i y_i^{true} K(\textbf x_i, \textbf x_{new}) + b$$

same thing in the minimizing condition:
$$1/2\sum_i \sum_j \alpha_i\alpha_j y_i^{true} y_j^{true} K(\textbf x_i, \textbf x_j) + \sum_k \alpha_k$$

sklearn's SVC predictor is capable of performing the kernel trick while linearSVC and SGDClassifier cant but are faster on linearly separable datasets.

### Other stuff

You can do regression with SVMs by fitting the decision boundary to the main shape of the  data and make the smallest street that contains all/most of the datapoints

common kernels
Linear: K a, b = a⊺b
Polynomial: K a, b = (γa⊺b + r)^d
Gaussian RBF: K a, b = exp (−γ∥ a − b ∥^2)
Sigmoid: K a, b = tanh(γa⊺b + r)