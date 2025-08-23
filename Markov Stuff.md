---
id: Markov Stuff
aliases: []
tags:
  - BasicMaths
---


### Markov chain
next state's probability distribution depends only on current state.
trajectory .
An MDP with fixed policy is a Markov chain in which the state is (state, action) pairs

### Markov decision process 
next state's probability distribution depends on current state and decision taken
[[Finite Markov Decision Process]]

### Hidden Markov models
observable state Y's probability distribution depends only on hidden state X that follows a Markov chain


## Mathematical implmentation
$$P(X_{n+1} = x_i | X_n = x_j) = H_{i,j}$$
thus 
$$P(X_{n+1} = x_i) = \sum_k H_{i,k}P(X_n = x_k)$$
to init the loop initial probabilities:
$$P(X_0 = x_i) = \pi_i$$
with MDP:
$$P(X_{n+1} = x_i | X_n = x_j , A_n = a_k) = H_{i,j,k}$$
$$P(X_{n+1} = x_i) = \sum_{k,j} H_{i,j,k}P(X_n = x_j)P(A_n = a_k)$$
with HMM:
$$P(X_{n+1} = x_i | X_n = x_j) = H_{i,j}$$
$$P(Y_{n} = y_i | X_n = x_j) = S_{i,j}$$

Always probability distributions are vectors and conditional probability distributions are matrices or higher order tensors which multiply the distribution vectors and allow for the computation of further probabilities
