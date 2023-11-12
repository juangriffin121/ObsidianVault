AlphaFold is a machine learning algorithm designed my the deepmind group to compete in the CASP protein folding prediction competition.
The algorithm takes an aminoacid sequence as an input and returns a 3D representation of all the residues of the protein.
The algorithm consists of three parts:
	[[Preprocessing]]
		Process the input sequence to generate numerical array features 
		that can be fed to the network and contain information of 
	[[Evoformer]]
		The main neural network of the algorithm. Returns output tensors of the same shape as its input and is fed back 3 times before returning its final output
	[[Structure Block]]
		Receives the output of the evoformer and uses it to construct the final 3D representation
During the training stage the algorithm performs [[Gradient Descent]] to optimize its parameters.
#done
![[AlphaFold.canvas]]

