Turning words into 512(d_model) dimension vectors
Steps:
	one hot encoding of the word
	learnt embedding (neural net from OneHot vector to embedding vector) [[Embedding]] of the words like word2vec that keeps meaning, (king - man + woman = queen) ([[Word embeddings]])
Positional encoding, after getting the vectors for each word in a sentence we add information about the position of the word in the sentence to those vectors, for that we add the positional encoder vector another 512 dimension vector that contains the information of the position of the word and the network could hopefully pick up when it learns.

the position encoding vector is obtained

$PE(pos)[2i] = sin(\frac {pos}{10000^{\frac{2i}{d_{model}}}})$

$PE(pos)[2i +1 ] = cos(\frac {pos}{10000^{\frac{2i}{d_{model}}}})$

$PE(pos + \Delta) - PE(pos) \not = PE(\Delta)$


Vector for word at pos = 5:
![[Pasted image 20231106183023.png]]
all position encoder vectors up to 1000 positions:
![[Pasted image 20231106183038.png]]
code for those images: UPLOAD IT TO GITHUBBBB
C:\Users\Usuario\my_scripts\PositionalEncoder.py

"We chose this function because we hypothesized it would allow the model to easily learn to attend relative positions, since for any fixed offset k, $PE_{pos+k}$  can be represented as a linear function of  $PE_{pos}$."
$$
PE_{pos + k} = PE_{pos} W_k
$$


$$
PE_{pos+k+j} = PE_{pos+k}W_j = PE_{pos}W_kW_j
$$


$$
W_{k+j} = W_kW_j
$$

$$
PE_{pos} = PE_0 W_1^{pos}
$$
$PE_0$ is 0 for even components and one for odd ones, the matrix W1 is learnable

