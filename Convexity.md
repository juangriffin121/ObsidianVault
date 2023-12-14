
The change of the gradient in the direction im moving always has a positive component in the same direction for any direction i choose.
the change in the gradient in my direction vector u is Hu where H is the hessian matrix of second derivatives

The Hessian of a function that is convex has a specific property: it is positive semidefinite. This means that for any non-zero vector  $\mathbf{u}$, the quadratic form given by $\mathbf{u}^T H \mathbf{u}$ is non-negative, where  $H$ is the Hessian matrix of the function. In simpler terms, this implies that the second derivative of the function (which the Hessian represents in multidimensional cases) is non-negative, a characteristic of convex functions

$$D_u\nabla f = u^j \frac{\partial^2 f}{\partial x^j \partial x^k}u^k$$

for the i basis vectors: $u^j = \delta_i^j$
$$\partial_i \nabla f = \delta^j_i \frac{\partial^2 f}{\partial x^j \partial x^k}\delta^k_i = \frac{\partial^2 f}{{\partial x^i}^2}$$
The diagonal needs to be positive and the hessian is always symetric
