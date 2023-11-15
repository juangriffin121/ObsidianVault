The [[Neural Networks]] in this project are coded as ordered lists of [[Layer Objects]]

In this project we use multiple different layers:
	[[Dense Layer object]]
	[[Convolutional Layer object]]

Then there's the function predict(respuesta) that takes in a network and computes the response of the network with the parameters it has at the moment from an input, it runs through the network in order and uses the output of the previous layer as the input to the previous, this is the function that is going to be used once the network is trained.

```python
def respuesta(red,Input):
  output = Input
  for capa in red:
    Input = output
    output = capa.forward(Input)
  return output 
```

The function train (entrenar) performs [[Gradient Descent]], it takes the network and the training dataset, it runs through the dataset, and with each data entry, it runs through the network forward calling the forward method to get all the activation values for each layer, and it the runs the network backwards calling the backward method to fit the parameters, this done in a number of iterations.

```python
def iteracion(red,datos,dt):
  for dato in datos:
    Input = dato['input']
    output_correcto = dato['output']
    output = respuesta(red,Input)
    grad_error = dev_costo(output, output_correcto)
    backpropagation(red,grad_error,dt)
  
def entrenar(red,datos,num_iteraciones):
  for i in range(num_iteraciones):
    iteracion(red,datos,0.2)
```