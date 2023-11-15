The implementation of the concept of [[Layers]] in my Wkiki project
A layer is an object that contains two methods, forward and backward

```python
class Capa:
  def __init__(self):
    pass
  def forward(self,Input):
    pass
  def backward(self,grad_output,dt):
    pass
```

Forward calculates the output to the layer from the input, both the output and input are ndarrays, the shape of both is inputed in the initialization of the object and the shapes they can accepts depend on the type of layer.

$\textbf{y} = l(\textbf{x},\theta) = l_{\theta}(\textbf{x})$

Backward calculates the gradient of the loss function with respect to the input as a function of gradient of the loss function with respect to the output with a contraction product between $\frac{dE}{dY}$ and $\frac{dY}{d\textbf{x}}$ that follows the chain rule for the particular shape of the arrays

$\frac{dE}{d\textbf{x}} = \frac{dE}{d\textbf{y}} \frac{d\textbf{y}}{d\textbf{x}} = \frac{dE}{d\textbf{y}} \frac{\partial l}{\partial \textbf {x}}(\textbf{x},\theta)$

the contraction product formula is this where $I$ is an index array like (i,j,k) for a 3D array

$\frac{dE}{d\textbf{y}} \cdot \frac{d\textbf{y}}{d\textbf{x}} = \sum \limits_{I = \vec{0}}^{Yshape}(\frac{dE}{d\textbf{y}_I}\frac{d\textbf{y}_I}{d\textbf{x}})$

here we have derivatives of a simple float with respect to an ndarray, its going to be an ndarray with the same shape as the original array, and derivatives of an ndarray with respect to another ndarray, this will result in a Y-shaped array of X-shaped arrays of the sort $\frac{dY_{I}}{dX}$.

The resulting shape would be (*shape(Y), *shape(X))

If the layer has any parameters the gradient of the loss function with respect to the parameter array as a function of gradient of the loss function with respect to the output and modifies the parameters following gradient descent

$\frac{dE}{d\theta} = \frac{dE}{d\textbf{y}} \frac{d\textbf{y}}{d\theta} = \frac{dE}{d\textbf{y}} \frac{\partial l}{\partial \theta}(\textbf{x},\theta)$

$\theta  = \theta - \lambda\frac{dE}{d\theta}$

