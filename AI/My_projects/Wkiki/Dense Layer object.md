
### Dense layer
A subclass of [[Layer Objects]] in which each component of the output vector (each output neuron)
is a linear combination of all the input vector's components(input neurons), this operation is computed by a matrix multiplication of the input with a weight matrix.
```python
class Densa(Capa):
  def __init__(self,output_size,input_size):
    self.pesos = np.random.randn(output_size,input_size)
    self.sesgos = np.random.randn(output_size,1)
    self.forma_output = (output_size,1)
  def forward(self,Input):
    self.input = Input
    return self.pesos @ self.input + self.sesgos
  def backward(self,grad_output,dt):
    grad_pesos = grad_output @ self.input.T
    grad_sesgos = grad_output
    grad_input = self.pesos.T @ grad_output
    self.pesos -= grad_pesos*dt
    self.sesgos -= grad_sesgos*dt
    return grad_input
```

For the dense layer the function $l$ calculates the vector output $y$ is a linear combination of its vector input $x$ with a vector bias

$y_j = \sum \limits _{i = 0} ^{N} w_{ji}\cdot x_i + b_j$

$\theta = (W,\vec{b})$

$\vec{y} = l(\vec{x},(W,\vec{b})) = W\vec{x} + \vec{b}$

```python
  def forward(self,Input):
    self.input = Input
    return self.pesos @ self.input + self.sesgos
```
___
with the functional form of the derivatives derived in [[Dense Layer]] we can find the needed gradients for the backward method
```python
  def backward(self,grad_output,dt):
    grad_pesos = grad_output @ self.input.T
    grad_sesgos = grad_output
    grad_input = self.pesos.T @ grad_output
    self.pesos -= grad_pesos*dt
    self.sesgos -= grad_sesgos*dt
    return grad_input
```

$$
\frac{dE}{d\textbf{x}} = \frac{dE}{d\textbf{y}} \frac{d\textbf{y}}{d\textbf{x}} 
= \frac{dE}{d\textbf{y}} \frac{\partial l}{\partial \textbf {x}}(\textbf{x},\theta) 
= W^T \cdot \frac{dE}{d\vec{y}}
$$
```python
grad_input = self.pesos.T @ grad_output
```
$$\frac{dE}{d\theta} = \frac{dE}{d\textbf{y}} \frac{d\textbf{y}}{d\theta} = \frac{dE}{d\textbf{y}} \frac{\partial l}{\partial \theta}(\textbf{x},\theta)$$

$$\frac{\partial E}{\partial \theta}: [\frac{\partial E}{\partial W},\frac{\partial E}{\partial \vec{b}}]$$

$$
\frac{dE}{dW} = \frac{dE}{d\textbf{y}} \frac{d\textbf{y}}{dW} = \frac{dE}{d\textbf{y}} \frac{\partial l}{\partial W}(\textbf{x},\theta) 
= (\frac{dE}{d\vec{y}})^T \cdot (I \otimes \vec{x}) = \frac{dE}{d\vec{y}}\vec{x}^T
$$
```python
grad_pesos = grad_output @ self.input.T
```
$$
\frac{dE}{d\vec{b}} = \frac{dE}{d\textbf{y}} \frac{d\textbf{y}}{d\vec{b}} = \frac{dE}{d\textbf{y}} \frac{\partial l}{\partial \vec{b}}(\textbf{x},\theta) 
= \frac{dE}{d\vec{y}} \cdot I = \frac{dE}{d\vec{y}}
$$
```python
grad_sesgos = grad_output
```
