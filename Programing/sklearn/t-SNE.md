Embedding algorithm used mostly for visualization purposes, 

We have a set of instance vectors $\vec x_i$ the algorithm consists on creating a probability for each pair of instances of being considered neighbors, given their high dimensional representation, the probability is given by the formula:

$$p_{ij} = \frac {exp(-d_{ij}^2)}{\sum_k exp(-d_{ik}^2)}$$
where $d_{ij}$ is the scaled Euclidean distance of the high dimensional representation.

$$d_{ij} = \frac{|\vec x_i - \vec x_j|}{2\sigma_i^2}$$

where σi is either set by hand or (as in some of our experiments) found by a binary search for the value of σi that makes the entropy of the distribution over neighbors equal to logk. Here, k is the effective number of local neighbors or “perplexity” and is chosen by hand.


In the low-dimensional space we also use Gaussian neighborhoods but with a fixed variance (which we set without loss of generality to be 1 2) so the induced probability qij that point i picks point j as its neighbor is a function of the low-dimensional images yi of all the objects and is given by the expression:

$$q_{ij} = \frac {exp(-|\vec x_i - \vec x_j|^2)}{\sum_k exp(-|\vec x_i - \vec x_k|^2)}$$
The aim of the embedding is to match these two distributions as well as possible. This is achieved by minimizing a cost function which is a sum of Kullback-Leibler divergences between the original (pij) and induced (qij) distributions over neighbors for each object:

$$\sum_i \sum_j p_{ij}log(\frac {p_{ij}}{q_{ij}}) = \sum_i KL(P_i||Q_i)$$
the sum over all instances of the KL divergence between he probability distributions given by the high dimensional space and the low dimensional space





