### Dense layer

For the dense layer the function $l$ calculates the vector output $y$ is a linear combination of its vector input $x$ with a vector bias

$y_j = \sum \limits _{i = 0} ^{N} w_{ji}\cdot x_i + b_j$

$\theta = (W,\vec{b})$

$\vec{y} = l(\vec{x},(W,\vec{b})) = W\vec{x} + \vec{b}$
___
and the two needed derivatives $\frac{\partial l}{\partial x}(\textbf{x},\theta)$ and $\frac{\partial l}{\partial \theta}(\textbf{x},\theta)$:

$\frac{\partial l}{\partial x}(\textbf{x},\theta)$:

$$
\frac{\partial y_j}{\partial x_i}(\textbf{x},\theta) 
= \frac{\partial}{\partial x_i}(\sum \limits _{k = 0} ^{N} w_{jk}\cdot x_k + b_j)
=\sum \limits _{k = 0} ^{N} w_{jk}\cdot \frac{\partial x_k}{\partial x_i}
=\sum \limits _{k = 0} ^{N} w_{jk}\cdot \delta_{ki}
=w_{ji}
$$

The object $\delta_{ki}$ is called the Kronecker delta in tensor calculus and its the entries of the identity matrix

$$\frac{\partial l}{\partial x}(\textbf{x},\theta) = W$$
___
$$\frac{\partial l}{\partial \theta}(\textbf{x},\theta): [\frac{\partial l}{\partial W}(\textbf{x},\theta),\frac{\partial l}{\partial \vec{b}}(\textbf{x},\theta)]$$

$\frac{\partial l}{\partial \vec{b}}(\textbf{x},\theta)$:

$$
\frac{\partial y_j}{\partial b_i}(\textbf{x},\theta)
=\frac{\partial}{\partial b_i}(\sum \limits _{k = 0} ^{N} w_{jk}\cdot x_k + b_j)
=\frac{\partial b_j}{\partial b_i}
= \delta_{ji}
$$

$$\frac{\partial l}{\partial \vec{b}}(\textbf{x},\theta) = I$$
___
$\frac{\partial l}{\partial W}(\textbf{x},\theta)$:

$$
\frac{\partial yj}{\partial w_{ik}}(\textbf{x},\theta)
=\frac{\partial}{\partial w_{ik}}(\sum \limits _{u = 0} ^{N} w_{ju}\cdot x_u + b_j)
=(\sum \limits _{u = 0} ^{N} \frac{\partial w_{ju}}{\partial w_{ik}} x_u)
=(\sum \limits _{u = 0} ^{N} \delta_{ij}\delta_{uk}\cdot x_u)
=(\delta_{ij} x_k)
$$

this last object is a 3D array which can be thought of as being the result of multiplying each entry of the identity matrix by the input vector, we'll represent this with the tensor product

$$\frac{\partial l}{\partial W}(\textbf{x},\theta) = I\otimes \vec{x}$$
___
Implemented in [[Dense Layer object]]