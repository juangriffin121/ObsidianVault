If i have a model distribution for a random variable X described by parameters $\theta$
the probability of observing a particular value for X with set parameters is:
$$p(X) = f(X,\theta)$$
The likelihood of a certain set of parameters given the data is the probability given to the observed data from those parameters:

$$L(\theta|X = X_0) = f(X_0,\theta)$$
The likelihood of the parameters given multiple observations is the probability of the multiple observations given by the models parameters. This is equal to the product of the individual probabilities.

$$L(\theta|\bigwedge \limits_i (X = X_i)) = \prod \limits_i f(X_i,\theta)$$
This is why log likelihood is usually preferred to make the products into sums and still be able to calculate maximum likelihood.
