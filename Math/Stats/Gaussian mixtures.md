A gaussian mixture model is a model for a random variable distribution that assumes that the distribution is a sum of n different gaussian distributions with different means, variance and density (the percentage of 1 the integral of each gaussian represents).

$$f(X,\theta) = \sum_i \alpha_i \frac {1}{\sqrt {2\pi} \sigma_i} e^{-\frac{1}{2}(\frac{X-\mu_i}{\sigma_i})^2}$$where $\theta_i = (\alpha_i, \mu_i, \sigma_i)$ and $\sum \limits_i \alpha_i = 1$

The optimization algorithm consists of two steps, the estimation step, where every instance is assigned a probability of belonging to each cluster, and then the maximization step where each cluster is modified by all the instances weighted by the probabilities that that instance belongs to that cluster