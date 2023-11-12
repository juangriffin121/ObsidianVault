## Objective
A generative adversarial network (GAN) is a machine learning algorithm to train a network (the [[Generator]]) to be able to generate images that are indistinguishable from the real dataset images.
## Structure
The algorithm works by coupling the generator with another network, the [[Discriminator]] whose job is to tell if an image is real or generated, the [[loss function]] for this network is the [[cross entropy loss]] between the probability given by the discriminator of the image being real and a boolean of is real (1 if true 0 if false), the loss function for the generator is actually the opposite, since its job is to create indistinguishable images, hence adversarial networks.
## Learning
For the [[Gradient Descent]] of the discriminator the gradient of the loss with respect to the output Z (probability of real) ($\frac {\partial E}{\partial Z}$) is directly used to calculate the gradient of the parameters ($\frac {\partial E} {\partial \Theta_D}$ )and it can be used to calculate the gradient of the gradient of the loss with respect to the discriminators input Y ($\frac {\partial E} {\partial Y}$) which, in the case of the generated images, is the output of the generator, since the loss function for the generator is the opposite of the discriminator's, the gradient with respect to the output of the generator which is then used to perform gradient decent on it is the negative of ($\frac {\partial E} {\partial Y}$).
#done 
![[Generative Adversarial Network.canvas|Generative Adversarial Network]]
