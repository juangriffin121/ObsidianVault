[[Embedding + Positional Encoding]]
[[Encoder (transformer)]]
[[Decoder (transformer)]]
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
This layer is where the majority of the parameters of the models live.
if x is the token `Michael` + `Jackson` (Jackson embedded with [[Attention]] with `Michael` as context ), there could be a row in $W_1$ (call that row vector $w_{i1}$) which learned to pick up on this particular direction, would then give a high signal, would pass the `ReLu` and then this new vector $p = W_1x$ (the i vector then produces a high signal on $p_i$) then creates a weighted sum over the columns of $W_2$, and the column in $W_2$ (call that column vector $w_{i2}$) corresponding to the high signal row in $W_1$ ($w_{i1}$) is highly scaled in the weighted average by $p_i$ 
$$p_i = f(\textbf x \cdot \textbf w_{i1})$$
$$\Delta \textbf x = p_i \textbf w_{i2} + rest$$
$$\textbf x = \textbf x + \Delta \textbf x$$
Due to the [[johnson-lidenstrauss]] lemma in a high dimensional spaces the amount of *almost perpendicular* directions increases exponentially with the dimensionality. This in this case means that with matrices $W_1$: `(d_model, 1000)` and $W_2$: `(1000, d_model)` while the model could have learned 1000 perfectly perpendicular where each row and column pair stores one concrete meaning, but it can also learn a space codified by 1000 basis vectors where $O(2^{1000})$ almost perpendicular directions within that space represent concrete real world meaning while the basis vectors dont represent one particular meaning, and the operation $y = W_2 f(W_1 \textbf x)$ is *almost equivalent* to the sum of those $O(2^{1000})$ meaningful directions weighted by the similarity of $\textbf x$ to each of them, this latter case is clearly a much lower minimum.       
[[Gradient Descent]]
![[Transformer.canvas]]