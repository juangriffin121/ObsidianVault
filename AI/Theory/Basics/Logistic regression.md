Regression algorithm that can be used for classification, estimate the probability that an instance belongs to a particular class.
Just like a linear regression model, a logistic regression model computes a weighted sum of the input features (plus a bias term), but instead of outputting the result directly like the linear regression model does, it outputs the logistic of this result.
$p = h_\theta(x) = \sigma(\theta^Tx)$
σ (t) = 1/( 1 + exp (− t))

binary [[Softmax]]

The loss function used is the [[Cross Entropy]] in this case is the log loss

− 1 m ∑i = 1 m y i log p i + 1 − y i log 1 − p i

The value of x at which $h(\theta^T x)$ is 0.5 is called the decision boundary.

>[!info]
>The log loss was not just pulled out of a hat. It can be shown mathematically (using Bayesian inference) that minimizing this loss will result in the model with the maximum [[Likelihood]] of being optimal, assuming that the instances follow a Gaussian distribution around the mean of their class(see [[Gaussian mixtures]]). When you use the log loss, this is the implicit assumption you are making. The more wrong this assumption is, the more biased the model will be. Similarly, when we used the MSE to train linear regression models, we were implic‐ itly assuming that the data was purely linear, plus some Gaussian noise. So, if the data is not linear (e.g., if it’s quadratic) or if the noise is not Gaussian (e.g., if outliers are not exponentially rare), then the model will be biased.