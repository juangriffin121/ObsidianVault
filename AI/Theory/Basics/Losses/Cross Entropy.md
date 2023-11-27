The excess "surprise" or information gain. when using predicted distribution q(x) and the real distribution is p(x). Its also a measure of the difference between p and q. The minimal value for the  cross entropy is equal to the [[Information Entropy]], and the difference between them is the [[KL divergence]].

$H(p,q) = E_p(\log(q)) = \sum \limits _x p(x)\log(\frac {1}{q(x)}) = -\sum p(x)\log(q(x))$

As a [[loss function]] it takes a predicted probability $\hat y$, a vector that assigns a probability to all possible outcomes, and compares it to the real probability, usually a [[one hot encoding]] vector.

$$
E = -\sum \limits_i y_i ln(\hat y_i)
$$
$$
\frac {\partial E}{\partial \hat y_k} = -\frac {y_k}{\hat y_k}
$$
When coupled with the [[Softmax]] activation function, a common choice because it creates a probability distribution, the calculations simplify.

$$
\frac {\partial E}{\partial x} = \hat y^T(I - \hat y) \frac {\partial E}{\partial \hat y}
$$
$$
\frac {\partial E}{\partial x_i} = \sum \limits_k \hat y_k(\delta_i^k - \hat y_i) \frac {\partial E}{\partial \hat y_k} = -\sum \limits_k \hat y_k(\delta_i^k - \hat y_i)\frac {y_k}{\hat y_k} = -\sum \limits_k (\delta_i^k y_k - \hat y_i y_k) = y_i - \sum \limits_k \hat y_i y_k
$$
And if $y$ is a one hot encoding vector the equation simplifies further to:
$$
\frac {\partial E}{\partial x_i} =  y_i - \hat y_i
$$


