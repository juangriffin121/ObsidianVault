
Neural networks are universal function approximators, for any kind of prediction problem where the data follows a very complex but not random functional correspondence between input and output, neural networks are a useful tool. 

When we have a dataset with entries of the form $(X,Y_{true})$

and the data follows some complex and unknowable functional relation $Y = F(X) + \epsilon$  with certain error due to variables not taken into account in the data or errors in the labeling process

The trained neural network as a black box works like this:

$Y = N(X,\Theta) = N_\Theta(X)$

It comes from a function of an input and parameters that when fitted to the data it best predicts the real output from the input. It takes an input X which has to be an n dimensional array of numerical values (for non numerical data there are ways of converting it into numerical form), and it has a set $\Theta$ of fitted optimizable numerical parameters which define the prediction of the network to any possible input, and it returns another array Y as an output.

N in this case is a known function whos output from a certain input is determined by the parameters, and with enough complexity and optimized parameters they can fit any functional form between the data

The parameters are tweaked in such a way to optimize a [[loss function]] $E(Y,Y_{true})$ for the values of Y in the dataset.

The particular functional form of the network and the way the parameters affect the output depends on the type of network used which may differ depending on the problem thats needed to solve.

A general property that machine learning algorithms that use neural network have is they are composed of [[Layers]], that allow the algorithm to have enough complexity to properly fit potentially complex data but has components that are simpler to deal with,  and they are concatenated in a way that the output of one is the input of the next one, so that when the input enters the first layer it runs through the network and returns an output out the last layer.

The layered shape of the network as well as the functional form of the loss allow neural networks to be very well fitted to be optimized by [[Gradient Descent]]

