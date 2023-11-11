## Full Network
Main network in AlphaFold
This block is a deep neural network that consists of 48 units that return the same shaped output: 
	An msa representation
	A pair representation
	Template pair information
	Template single information
This tensors are computed Nensemble times with stochastic variation in the methods (different cllustering, order in sequences, deletions) in the ==preprocessing== stage and the ==outputs== of the different ensembles are then averaged
The full network is also run three times (Ncycle) with the same input, feeding its output back onto the network, before returning its final output to the [[Structure Block]].

## Evoformer Unit

The network is fed an msa representation $\textbf m_{ij}$ (an embedded form of the msa from preprosessing in which each position in the alignment in esch cluster is a vector pf 256 dimension) of shape: (Nclust, Nres, 256) and a pair representation $\textbf z_{ij}$ (an embedded form of pairs of positions in the input secuence) of shape: (Nres,Nres,128) and ==other tensors==, the msa is processed in an [[MSA row-wise gated self-attention with pair bias]] layer that linearly combines the position matrices(vector of vectors of shape: (Nclust,256)), that represent a position in the alignement in all the clusters, with weights determined by the similarity(dot product) between pairs of positions, since the dot product between pairs is a matrix of shape: (Nres,Nres) the pair representation is added as a bias to that matrix before continuing with the attention algorithm.
A similar procedure is later done with pairs of cluster sequences instead of positions in the [[MSA column-wise gated self-attention]] that also returns an msa reoresentation without biasing with the pair representation.
