A non-parametrized layer that returns a vector that adds up to one with only positive values, basically a probability distribution that keeps the order of the original layer values and has a simple derivative.
$$
y = softmax(x): y_i = \frac{e^{x_i}}{\sum \limits_{j}e^{xj}} 
$$
#### gradients:
$$
\frac {\partial E}{\partial x_i} = \frac {\partial E}{\partial y_k}\frac {\partial y_k}{\partial xi}  
$$

$$
\frac {\partial y_k}{\partial x_i} = \frac {\partial}{\partial xi} \frac{e^{x_k}}{\sum \limits_{j}e^{xj}} = \frac {\partial}{\partial x_i} (e^{x_k})(\sum \limits_{j}e^{xj})^{-1} = \frac {\partial e^{x_k}}{\partial x_i}(\sum \limits_{j}e^{xj})^{-1} + e^{x_k} \frac {\partial}{\partial xi} (\sum \limits_{j}e^{xj})^{-1}
$$
$$
\frac {\partial y_k}{\partial x_i} = \frac {\partial e^{x_k}}{\partial x_i}(\sum \limits_{j}e^{xj})^{-1} + e^{x_k} \frac {\partial}{\partial xi} (\sum \limits_{j}e^{xj})^{-1} = \delta_i^k e^{x_k} (\sum \limits_{j}e^{xj})^{-1} + e^{x_k} (-1)(\sum \limits_{j}e^{xj})^{-2}\frac {\partial e^{x_i}}{\partial xi} = \delta_i^k\frac{ e^{x_k}} {(\sum \limits_{j}e^{xj})} - \frac{e^{x_k} e^{x_i}}{ (\sum \limits_{j}e^{xj})^2} 
$$
$$
\frac {\partial y_k}{\partial x_i} = \delta_i^k\frac{ e^{x_k}} {(\sum \limits_{j}e^{xj})} - \frac{e^{x_k} e^{x_i}}{ (\sum \limits_{j}e^{xj})^2} = \delta_i^ky_k - y_k y_i = y_k(\delta_i^k - y_i)
$$
$$
\frac{\partial y}{\partial x} = y^T(I - y)
$$
$$
\frac {\partial E}{\partial x} = y^T(I - y) \frac {\partial E}{\partial y}
$$
When coupled with the [[Cross Entropy]] loss function, a common choice because it compares a predicted probability distribution with the true one, the calculations simplify.

$$
\frac {\partial E}{\partial \hat y_k} = -\frac {y_k}{\hat y_k}
$$
Where $\hat y$ is the predicted probability vector output of the SoftMax and $y$ is the target distribution
$$
\frac {\partial E}{\partial x_i} = \sum \limits_k \hat y_k(\delta_i^k - \hat y_i) \frac {\partial E}{\partial \hat y_k} = -\sum \limits_k \hat y_k(\delta_i^k - \hat y_i)\frac {y_k}{\hat y_k} = -\sum \limits_k (\delta_i^k y_k - \hat y_i y_k) = y_i - \sum \limits_k \hat y_i y_k
$$
And if $y$ is a [[one hot encoding]] vector the equation simplifies further to:
$$
\frac {\partial E}{\partial x_i} =  y_i - \hat y_i
$$
This basically boils down to saying the correct output should be closer to 1 and the rest closer to zero, ie you "tell" the network it should modify the weights in the way that makes them change in the direction that minimizes the difference vector between the prediction and the one hot encoding. 

