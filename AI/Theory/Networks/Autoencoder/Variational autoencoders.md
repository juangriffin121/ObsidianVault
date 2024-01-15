This is a modification of the [[Autoencoders]] that intends to capture a distribution on the [[Latent space]] of the input instead of just a vector, to do this, the encoder returns two vectors of the latent space, the mean and the  log_variance(since variance is positive definite, linear dense layers tend to return signed values and it makes sense to use the exp function to get the variance value from the output of the encoder).
The error function contains the difference between the mean output and the input but also a component [[KL divergence]] between the distribution(mu, sigma) and a standard normal distribution N(0,1).
From the way we sample we have a covariance matrix $\Sigma$ thats diagonal, so we only need the variance vector $\vec \sigma$ and given:
$$D_{KL}(N(\vec \mu, \vec \sigma), N(\vec 0, \vec 1)) = \sum_i 1 + log(\sigma_i^2) - \mu_i^2 - \sigma_i^2 $$
This term has the effect of forcing the network to make the latent space vector components to be more independent of each other and makes them in some cases reflect human understandable properties of the input, like in the example of images they can represent rotations, lightning, face emotions etc, and modifying one and leaving the rest the same can have the effect of only modifying that property keeping the rest constant. This effect can be tweaked by scaling the divergence term by a hyperparameter.

So the loss has two components, the MSA for the reconstruction loss and the KL divergence of the latent distribution.
The later can only affect the gradients of the encoder, since the layers of the decoder come after them and so don't have influence over the values of the mean and variance.
This component of the loss function is added onto the gradient of the reconstruction loss at the sampling layer:

Sampling layer:
	Forward:
$$\mu, z = x$$
$$\sigma^2 = e^z$$
$$\sigma = e^{\frac z2}$$
$$y = \mu + \epsilon\sigma$$
	Backward:
$$\frac {dE}{dx} = (\frac{dE}{d\mu}, \frac{dE}{dz})$$
$$\frac{dE}{d\mu} = \frac {dE}{dy}$$
$$\frac {dE}{dz} = \frac {dE}{dy}\epsilon\frac{d\sigma}{dz} = \frac {dE}{dy}\epsilon \frac{e^{\frac z2}}2 = \frac {dE}{dy}\epsilon \frac\sigma2$$
Those are the gradients that come from the reconstruction loss, here we will add the component that comes from the [[KL divergence]].
$$E_2 = -1/2\sum_i 1 + log(\sigma_i^2) - \mu_i^2 - \sigma_i^2$$
$$E_2 = -\frac 12\sum_i 1 + z_i - \mu_i^2 - e^{z_i}$$
$$\frac{dE_2}{d\mu} = \mu$$
$$\frac{dE_2}{dz_j} = \sum_i -\frac{dz_i}{dz_j} + \frac {de^{z_i}}{dz_j} = \sum_i \delta^i_j(-1 + e^{z_i}) = \frac{-1 + e^{z_j}}2$$
$$\frac {dE_2}{dz} = \frac{-1 + e^z}2$$
