Layer normalization is a technique used in deep neural networks to stabilize gradients which in the absence of normalization they tend to diminish.
The procedure is, after getting a vector $X$ from previous layers (see what happens with higher tensors) you compute the mean of all the components of the vector $\mu$ and the standard deviation $\sigma$  and normalize ([[Scaling#^d3ad36]]) all the components with this values, ie the components after the normalization are, how many standard deviations is this component away from the mean of the components.
$$\mu = \frac{1}{N}\sum \limits_{i = 0}^{N}X_i$$
$$\sigma = \sqrt{\frac{1}{N}\sum \limits_{i = 0}^{N}(X_i-\mu)^2}$$

$$X'_i = \frac{X_i - \mu}{\sigma}$$
And then the output is calculated by applying a simple linear function with two learnable parameters.
$$Y_i = \gamma X'_i + \beta$$
For every component we use the same parameters.

Backpropagation
We have $\frac {\partial E}{\partial Y_i}$ then 
$$\frac {\partial E}{\partial X_j} = \frac {\partial E}{\partial Y_i}\frac {\partial Y_i}{\partial X'_k}\frac {\partial X'_k}{\partial X_j} = \frac {\partial E}{\partial Y_i}\gamma \delta_{k}^i \frac {\partial X'_k}{\partial X_j} = \frac {\partial E}{\partial Y_i}\gamma \frac {\partial X'_i}{\partial X_j} = \frac {\partial E}{\partial Y_i}\gamma \frac{\partial \frac{X_i - \mu}{\sigma}}{\partial X_j}$$
$$d(\frac{f}{g}) = \frac {g\cdot df - f\cdot dg}{g^2}$$
$$\frac{\partial \frac{X_i - \mu}{\sigma}}{\partial X_j} = \frac{\sigma\frac{\partial (X_i - \mu)}{\partial X_j} - (X_i - \mu) \frac{\partial \sigma}{\partial X_j}}{\sigma^2}$$
$$\frac{\partial (X_i - \mu)}{\partial X_j} = \frac{\partial X_i}{\partial X_j} + \frac{\partial \mu}{\partial X_j} = \delta_j^i + \frac{1}{N}$$
$$\frac{\partial \sigma}{\partial X_j} = \frac{1}{2\sigma}\frac{1}{N}\frac{\partial \sum \limits_{i = 0}^{N}(X_i-\mu)^2}{\partial X_j} = \frac{1}{2N\sigma}\sum \limits_{i = 0}^{N}\frac{\partial(X_i-\mu)^2}{{\partial X_j}} = \frac{1}{2N\sigma}\sum \limits_{i = 0}^{N}2(X_i-\mu)\frac{\partial(X_i-\mu)}{{\partial X_j}}  = \frac{1}{2N\sigma}\sum \limits_{i = 0}^{N}2(X_i-\mu)(\delta_j^i + \frac{1}{N})$$
$$\frac{1}{2N\sigma}\sum \limits_{i = 0}^{N}2(X_i-\mu)(\delta_j^i + \frac{1}{N}) = \frac{1}{2N\sigma}\sum \limits_{i = 0}^{N}2(X_i-\mu)\delta_j^i + \frac{2}{N}(X_i-\mu) = \frac{1}{2N\sigma}(2(X_j-\mu) + \sum \limits_{i = 0}^{N}\frac{2}{N}(X_j-\mu)) = \frac{X_j-\mu}{N\sigma} + \frac{2}{N}\sum \limits_{i = 0}^{N}(X_j-\mu) = \frac{1}{N}X'_j + 0$$
$$\frac{\partial \sigma}{\partial X_j} = \frac{1}{N}X'_j$$
$$\frac{\partial X'_i}{\partial X_j} = \frac{\sigma(\delta_j^i + \frac{1}{N}) - (X_i - \mu) \frac{1}{N}X'_j}{\sigma^2}$$


![[Layer Norm.canvas]]