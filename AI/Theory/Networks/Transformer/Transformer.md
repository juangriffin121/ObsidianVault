[[Embedding + Positional Encoding]]
[[Encoder]]
[[Decoder]]
#### FeedForward Network
The feedforward step is composed of two non biased [[Dense Layer]] with a non linear activation function in between them and the output of it is added to its input.
$$
FFN(x) = f(xW_1)W_2
$$
$$
x = x + FFN(x)
$$
The shape of the matrices are (dmodel, hidden_size) and (hidden_size,dmodel), the fact that they have columns or rows of size dmodel gives us a hint that they might represent sets of word vectors or that they pick them up, and that is the case, in fact, if we take x to be just one token vector, each of the rows of W1 picks up meaningful concepts, meaning that the value of their dot products is high with particular words that signify the same meaning (base and bases) , or in deeper layers they pick more semantic concepts that depend on multiple words on the sentence (as a part of, one of many, ...).
And the columns of W2 are then linearly combined based on the match between x and the rows of W1, in that sence the matrices work very similar to keys and values with the query x, and the columns of W2 tend to represent words related to the ones picked up by W1 and move the x vector towards creating a new token.
$$
FFN(x) = f(xK^T)V
$$

[[Gradient Descent]]
![[Transformer.canvas]]