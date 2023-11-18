Autoencoders are a [[Neural Networks]] architecture designed to compress high dimensional data into a small dimensional representation.
The network is separated in two parts, the encoder which is an architecture in which the [[Layers]] get increasingly lower dimensional as we get deeper into the network until we reach the bottle neck, the smallest dimensional representation of the data, this vector is an element of what is called the [[Latent space]] of the dataset. And from then theres an opposite architecture, the decoder, whose layers get increasingly bigger reaching the exact same shape of the output which, when trained, the network will return a very close aproximation of the input, the detail of the representation will depend on the dimension of the latent space.
The [[loss function]] is the mean squared difference with the input.

![[Autoencoder.canvas|Autoencoder]]
