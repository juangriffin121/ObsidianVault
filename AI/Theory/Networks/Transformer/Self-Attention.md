Allows the model to relate words to each other.
This algorithm is based on the [[Attention]] algorithm using the same input matrix as the query key and value matrices.
With a sentence of seq_len words it can be represented as a matrix S of shape (seq_len,d_model), a sequence of word vectors, we make 3 copies of it Q(query), K(Key) and V(value)
Q, K, V = S, S, S
With the matrix Q and K we can create a new matrix $Q K^{T}$ which has shape (seq_len, seq_len) and its (i,j)th component is the dot product between the vector for the ith word with the one for the jth and  given that word embedding spaces have meaningfull distance mesures the dot product of two words is a mesure of how important the relation between them is
this matrix is then passed through the [[Softmax]] function which ensures that each row(or column since its symetric) sums to 1 and scale by  $\frac {1}{\sqrt {d_{model}}}$ we get a relation matrix R that containts information about the relation between each word in the sentence, and we then modify the original S (V) matrix with this relation matrix so that the vector for the ith word is a linear combination of the vectors of all the other words with weights given by the R matrix getting the final A matrix:
$A_{cat} = \sum \limits _{word}^{len(seq)}(R_{cat,word}V_{word})$
$A = \frac {Softmax(Q K^{T})}{\sqrt {d_{model}}}V$
This layer doesnt have any learnt parameters.

![[Pasted image 20231106190649.jpg]]
![[Pasted image 20231106190737.jpg]]
The words "move" in the direction of the words related to it.
If i have apple and banana in a sentence, its vector will move closer to banana(new_apple = 0.7×apple +0.1×banana +...) and moving in the fruit direction will move it away from the tech brands.
new_apple-apple = new_apple - (.7apple +.1apple+...) = (0.7×(banana-apple)+...)
It moves in the direction of the weighted sum of all the difference vectors between themselves and apple. The difference between banana and apple is probably very similar to the difference between fruits and tech companies.
With [[Multi-head Attention]] different heads can pick the different meanings of the words and can pick the tech side of apple and forget about  banana while another hesd does the opposite.
Make chatgpt make a pun with the different meanings of apple