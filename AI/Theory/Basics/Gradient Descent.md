# $\vec{\nabla}\downarrow$

Gradient descent is a parameter optimization algorithm for function approximation, it works by defining a [[loss function]] that compares the correct output to the prediction of the aproximator based on the input given, in some cases the algorithm uses all the dataset in one iteration, in others it uses only one or a small batch data entry per iteration ([[Stochastic gradient descent]]).
With the data used for the iteration it calculates the [[Gradient]] of the loss function with respect to the aproximator's parameters and uses that gradient to move in the opposite direction (changing all the parameters in the most optimal way supposing linearity of the loss), the direction of steepest decrease.
In order to be able to perform this algorithm the loss function must be differentiable (and with an explicit formula) with respect to the aproximator's output, and so should the aproximators output be with respect to the parameters.
$\textbf Y_{true} = F(\textbf X) + \epsilon$
$\textbf Y = \Phi(\textbf X,\Theta)$
$loss = E(Y,Y_{true})$
$grad: \frac {\partial E} {\partial \Theta} =  \frac {\partial E} {\partial Y} \cdot  \frac {\partial Y} {\partial \Theta}$
$\Theta  = \Theta - \frac {\partial E} {\partial \Theta} \cdot LearningRate$

The algorithm is usually used in approximators with multiple [[Layers]] such as [[Neural Networks]] which allow the algorithm to have complexity to be able to fit complex data but keep it able to obtain explicit derivatives.

A common procedure in training with stochastic gradient descent is to use a "Learning schedule", a function of the number of iteration that makes the learning rate get smaller as the iterations go on, this is useful for making sure the model gets closer quickly to the minima but as time goes on it converges and slows down.
![[Gradient Descent.canvas]]