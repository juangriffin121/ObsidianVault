In the [[Transformer]]  network in the [[Attention]] step of each layer the following formula is computed (ignoring multihead attention for simplicity): 
$$softmax(Q K^T)V$$
$$Q = W_Q X$$
$$K = W_K X$$
$$V = W_V X$$
$Q$ $K$ and $V$ are learned aspects of the tokens X (X is the list of input tokens in that layer, they come from the input tokens to the network, at each layer these tokens get embedded with information from other tokens and learned data by the model) They are used to pick up on particular meanings in the tokens (apple fruit vs apple company). 
The attention algorithm for a particular token $\textbf x_i$ represents the weighted average of the values weighted by how much their corresponding keys are similar to that token. During training all the tokens are known so X is full so the computation can be done all at once with the matrix X.
During use, the matrix X consists only of the prompt tokens and then grows by one token while the model generates the response. This becomes a problem if computed naively because to compute the next token the model needs all the previous token vals and keys to compute attention, if not stored they have to be computed again, those previous tokens arent affected by future tokens by design so they wont change in the next computation, meaning that recomputing them is redundant and unnecesary, caching then becomes very important. 
Let $X(t)$ be the list of all tokens at time $t$ and $\textbf y$ be the next token generated and $\textbf x$ the last token `X(t)[-1]`.
`X(t + 1) = concat(X(t), y)`
let $\textbf q$, $\textbf k$, $\textbf v$ be the query key and value for the last token
$\textbf q =W_Q \textbf x$
$\textbf k =W_K \textbf x$
$\textbf v =W_V \textbf x$
let $K(t)$ and $V(t)$ be the cached key and value matrices at time t
$K(t+1) = concat(K(t), \textbf k)$
$V(t+1) = concat(V(t), \textbf k)$
we can now compute the attented token from $\textbf x$ 
$$softmax(\textbf q K(t + 1)^T)V(t+1)$$
and run through the network and do the same for all attention layers to get $\textbf y$ and go back
