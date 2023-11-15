#### Convolutional
```python
class Convolucional(Capa):
  def __init__(self,forma_input,tamano_filtro,num_filtros):
    profundidad_in,altura_in,ancho_in = forma_input
    self.forma_input = forma_input
    self.forma_filtro = (num_filtros,profundidad_in,tamano_filtro,tamano_filtro)
    self.forma_output = (num_filtros , altura_in - tamano_filtro + 1 , ancho_in - tamano_filtro + 1 )
    self.filtros = np.random.randn(*self.forma_filtro)
    self.sesgos = np.random.randn(*self.forma_output)
  def forward(self,Input):
    self.input = Input
    output = np.zeros(self.forma_output)
    for n,filtro3d in enumerate(self.filtros):
      for m,filtro in enumerate(filtro3d):
        output[n] += signal.correlate2d(self.input[m],filtro,mode = 'valid') + self.sesgos[n]
    return output
  def backward(self,grad_output,dt):
    self.grad_sesgos = grad_output
    self.grad_filtros = np.zeros(self.forma_filtro)
    grad_input = np.zeros(self.forma_input)
    for n,filtro3d in enumerate(self.filtros):
      for m,filtro in enumerate(filtro3d):
        self.grad_filtros[n][m] = signal.correlate2d(self.input[m],grad_output[n],mode = 'valid')
        grad_input[m] = signal.convolve2d(grad_output[n],filtro,'full')
    self.sesgos -= dt*self.grad_sesgos
    self.filtros -= dt*self.grad_filtros
    return grad_input
  
```
forward method:

$$
\textbf{y} = \textbf{K} (\cdot*) \textbf{x} + \textbf{b}
$$

$$y_{nij} =  \sum \limits_{m} [\textbf{k}_{nm}*\textbf{x}_m]_{ij} + b_{nij}
=\sum \limits_{m} (\sum \limits_{(a,b) = (0,0)}^{(KShape)} (x_{m,(i-{\lfloor}\frac{L}{2}{\rfloor} + a,j-{\lfloor}\frac{L}{2}{\rfloor} + b)} \cdot k_{nmab})) + b_{nij}
$$
```python
def forward(self,Input):
    self.input = Input
    output = np.zeros(self.forma_output)
    for n,filtro3d in enumerate(self.filtros):
      for m,filtro in enumerate(filtro3d):
        output[n] += signal.correlate2d(self.input[m],filtro,mode = 'valid') + self.sesgos[n]
    return output
```
___

with the gradient functions derived in [[Convolutional Layer]] we can perform the backward method:
```python
def backward(self,grad_output,dt):
    self.grad_sesgos = grad_output
    self.grad_filtros = np.zeros(self.forma_filtro)
    grad_input = np.zeros(self.forma_input)
    for n,filtro3d in enumerate(self.filtros):
      for m,filtro in enumerate(filtro3d):
        self.grad_filtros[n][m] = signal.correlate2d(self.input[m],grad_output[n],mode = 'valid')
        grad_input[m] = signal.convolve2d(grad_output[n],filtro,'full')
    self.sesgos -= dt*self.grad_sesgos
    self.filtros -= dt*self.grad_filtros
    return grad_input
  
```

grad input:
$$
\frac{\partial E}{\partial x_{l}}
=\sum \limits_{n = 0}^{feats} (\frac{\partial E}{\partial y_{n}} * k_{nl})
$$
```python
grad_input = np.zeros(self.forma_input)
for n in ...:
	grad_input[m] += signal.convolve2d(grad_output[n],filtro,'full')
```
full tensor:
$$
\frac{\partial E}{\partial \textbf x}
=\frac{\partial E}{\partial \textbf y} (\cdot * ) \textbf K
$$
grad kernels:
$$
\frac{\partial E}{\partial k_{nm}}
= \frac{\partial E}{\partial y_{m}} * x_{n}
$$
```python
self.grad_filtros[n][m] = signal.correlate2d(self.input[m],grad_output[n],mode = 'valid')
```
full tensor:
$$
\frac{\partial E}{\partial \textbf K}
= \frac{\partial E}{\partial \textbf y} (\otimes *) \textbf x
$$
grad bias:
```python
self.grad_sesgos = grad_output
```
$$
\frac{\partial E}{\partial \textbf b} = \frac{\partial E}{\partial \textbf y} 
$$
___

With those arrays computed we have everything needed to give the result to the next layer to do its forward, actualize this layers parameters and give the previous layer the output gradient needed for its backward method
```python
self.sesgos -= dt*self.grad_sesgos
self.filtros -= dt*self.grad_filtros
return grad_input
```
