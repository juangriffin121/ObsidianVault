#### First step

$$W = argmin((\sum_{i = 1}^{m} \vec x_i - \sum_{j = 1}^{k} w_{ij}\vec x_j)^2)$$
The instance vector x_i is aproximated as a weighted average of its k nearest neighbors, the weights are learned that minimizes the error.

the $w_{ij}\vec x_j$ is not a matrix multiplication.

#### Second step
With the weights of the neighbors of each vector to itself frozen, we turn to a smaller dimensional space, and what we modify this time is the coordinates of each low dimensional representation of the instances so that the difference between the weighted average of the neighbors and the vector are minimized, that way the low dimensional representation of the instance is closest to its nearest neighbors in that space aswell.

$$Z =  argmin(\sum_{i=1}^{m}\vec z_i - \sum _{j = 1}^{k}w_{ij} \vec z_j)$$

#### The gradients

Not really used, cause the solution is closed

$$E = \sum_i ||\vec x_i - \sum_j(w_{ij} m_{ij} \vec x_j)||^2 = \sum_i ||\vec x_i - \hat x_i||^2$$
$$E = tr((X - (W \odot M)X)(X - (W \odot M)X)^T)$$
where $m_{ij} = 1$ if j is one of the knn of i, and zero otherwise 


since we have the constraint of $\sum \limits_j m_{ij} w_{ij} = 1$

Gradient descent with constraints:
$$\frac {\partial \vec x}{\partial t} = \frac {\partial f}{\partial \vec x} - \frac{\frac {\partial f}{\partial \vec x} \cdot \frac {\partial g}{\partial \vec x}}{ ||\frac {\partial g}{\partial \vec x}||^2} \frac {\partial g}{\partial \vec x}$$
Thus, our change in w should be:

$$ \frac {\partial \vec w_i}{\partial t} = \frac {\partial E}{\partial \vec w_i} - \frac {\frac {\partial E}{\partial \vec w_i} \cdot \frac {\partial g}{\partial \vec w_i}}{||\frac {\partial g}{\partial \vec w_i}||^2} \frac {\partial g}{\partial \vec w_i}$$

$$ \frac {\partial w_{ij}}{\partial t} = \frac {\partial E}{\partial w_{ij}} - \frac {\frac {\partial E}{\partial \vec w_i} \cdot \frac {\partial g}{\partial \vec w_i}}{||\frac {\partial g}{\partial \vec w_i}||^2} \frac {\partial g}{\partial w_{ij}}$$

$$ \frac {\partial w_{ij}}{\partial t} = \frac {\partial E}{\partial w_{ij}} - \frac {\frac {\partial E}{\partial \vec w_i} \cdot \frac {\partial g}{\partial \vec w_i}}{n} m_{ij}$$ 

$$ \frac {\partial W}{\partial t} = \frac {\partial E}{\partial W} - \frac {diag(\frac {\partial E}{\partial W} \frac {\partial g}{\partial W}^T)}{n} \odot \frac {\partial g}{\partial W}$$ 
$$ \frac {\partial W}{\partial t} = \frac {\partial E}{\partial W} - \frac {diag(\frac {\partial E}{\partial W} \frac {\partial g}{\partial W}^T)}{n} \odot M$$ 

$$ \frac {\partial W_{ij}}{\partial t} = \frac {\partial E}{\partial W_{ij}} - \frac {diag(\frac {\partial E}{\partial W} \frac {\partial g}{\partial W}^T)_{i}}{n} \frac {\partial g}{\partial W_{ij}}$$
##### second step
$$E = (\sum_{i = 1}^{m} (\vec z_i - \sum_{j = 1}^{k} w_{ij}\vec z_j)^2)$$


$$\frac{\partial E}{\partial z_{ab}} = -2
(\sum_{i = 1}^{m} \vec z_i - \sum_{j = 1}^{k} w_{ij}\vec z_j) \cdot \frac {\partial (\sum_i( \sum_n z_{in} \vec e_n -  \sum_jw_{ij} \sum_n z_{jn} \vec e_n))}{\partial z_{ab}}$$
$$\frac{\partial E}{\partial z_{ab}} = -2
(\sum_{i = 1}^{m}( \vec z_i - \sum_{j = 1}^{k} w_{ij}\vec z_j)) \cdot  (\sum_i( \sum_n \frac{\partial {z_{in}}}{\partial z_{ab}} \vec e_n -  \sum_jw_{ij} \sum_n \frac{\partial {z_{jn}}}{\partial z_{ab}} \vec e_n))$$
$$\frac{\partial E}{\partial z_{ab}} = -2
(\sum_{i = 1}^{m}( \vec z_i - \sum_{j = 1}^{k} w_{ij}\vec z_j)) \cdot  (\sum_i( \sum_n \delta_{ia} \delta_{nb} \vec e_n -  \sum_jw_{ij} \sum_n \delta_{ja} \delta_{nb} \vec e_n))$$
$$\frac{\partial E}{\partial z_{ab}} = -2
(\sum_{i = 1}^{m}( \vec z_i - \sum_{j = 1}^{k} w_{ij}\vec z_j)) \cdot  (\sum_i( \delta_{ia} \vec e_b -  \sum_jw_{ij} \delta_{ja} \vec e_b))$$
$$\frac{\partial E}{\partial z_{ab}} = -2
(\sum_{i = 1}^{m}( \vec z_i - \sum_{j = 1}^{k} w_{ij}\vec z_j)) \cdot  (\vec e_b -  \sum_i w_{ia} \vec e_b)$$
$$\frac{\partial E}{\partial z_{ab}} = -2
(\sum_{i = 1}^{m}( \vec z_i - \sum_{j = 1}^{k} w_{ij}\vec z_j)) \cdot  (1 -  \sum_i w_{ia}) \vec e_b$$
$$\frac{\partial E}{\partial z_{ab}} = -2
(\sum_{i = 1}^{m}( \vec z_i \cdot \vec e_b - \sum_{j = 1}^{k} w_{ij}\vec z_j) \cdot \vec e_b)(1 -  \sum_i w_{ia})$$
$$\frac{\partial E}{\partial z_{ab}} = -2
(\sum_{i = 1}^{m} z_{ib} - \sum_{j = 1}^{k} w_{ij} z_{jb})(1 -  \sum_i w_{ia})$$



