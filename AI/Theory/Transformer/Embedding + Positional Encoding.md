Turning words into 512(d_model) dimension vectors
Steps:
	one hot encoding of the word
	learnt embedding (neural net from OneHot vector to embedding vector) [[Embedding]] of the words like word2vec that keeps meaning, (king - man + woman = queen)
Positional encoding, after getting the vectors for each word in a sentence we add information about the position of the word in the sentence to those vectors, for that we add the positional encoder vector another 512 dimension vector that contains the information of the position of the word and the network could hopefully pick up when it learns.

the position encoding vector is obtained
$PE(pos)[2i] = sin(\frac {pos}{10000^{\frac{2i}{d_{model}}}})$

$PE(pos)[2i +1 ] = cos(\frac {pos}{10000^{\frac{2i}{d_{model}}}})$

Vector for word at pos = 5
![[Pasted image 20231106183023.png]]
all position encoder vectors up to 1000 positions
![[Pasted image 20231106183038.png]]
code for those images: UPLOAD IT TO GITHUBBBB
C:\Users\Usuario\my_scripts\PositionalEncoder.py
#done 

