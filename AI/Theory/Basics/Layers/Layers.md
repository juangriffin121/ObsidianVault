Each layer has its own input and output and can have a subset of the parameters that define its output from the input, and they are concatenated in a way that the output of one is the input of the next one.

$\textbf{y}_i =  l(\textbf{x}_i,\theta) = l_\theta(\textbf{x}_i)$

$\textbf{x}_{i+1} = \textbf{y}_i$

$\textbf{x}_0 = X$

$\textbf{x}_n = Y$

The layers allow the algorithm to have enough complexity to properly fit potentially complex data but has components that are simpler to deal with and there are many well defined types of layers which we'll talk about that are better suited for different purposes and they can be mixed in the network

The layered structure also helps in calculating the derivatives to perform [[Gradient Descent]], if the functional form of the particular layer has a simple formula for its derivative with respect to its parameters the derivative of the loss can be easily calculated with the chain rule.

$\frac{dE}{d\theta} = \frac{dE}{d\textbf{y}} \frac{d\textbf{y}}{d\theta} = \frac{dE}{d\textbf{y}} \frac{\partial l}{\partial \theta}(\textbf{x},\theta)$

and to get each $\frac{dE}{dy}$ for each layer we can calculate with the chain rule $\frac{dE}{dx}$ which will be the new gradient needed for the previous layer in a process called backpropagation

$\frac{dE}{d\textbf{x}} = \frac{dE}{d\textbf{y}} \frac{d\textbf{y}}{d\textbf{x}} = \frac{dE}{d\textbf{y}} \frac{\partial l}{\partial \textbf {x}}(\textbf{x},\theta)$

The first $\frac{dE}{dy}$ comes from the output of the entire network $Y$ and the $\frac{dE}{dY}$ should be obtainable from the formula for $E(Y_{true},Y)$

Therefore along with the explicit formula for $l$, in order to be able to optimize the layer we need to be able to get an explicit formula for $\frac{\partial l}{\partial x}(\textbf{x},\theta)$ and $\frac{\partial l}{\partial \theta}(\textbf{x},\theta)$, having those for every layer, together with the formula for the derivative of the error with respect to the predicted output of the network $\frac{dE}{dY}$, we are able to perform the gradient descent method to fit the full network to the data