In a bandit problem: n possible actions each with its own reward distribution and only one state we can solve it by using a stochastic gradient ascent method, in order to do that we create n learnable parameters the preferences of each action and we determine the policy or probability of taking each action with a softmax of the preferences.
$$
P(A = a_i) = \frac {\exp(h_i)}{\sum_i \exp(h_j)} = \pi(a_i)
$$
the optimizing function is the expected reward of the action
$$
E[R] = \sum_i \pi(a_i)q(a_i)
$$
the gradient is then:
$$
\frac{\partial E}{\partial h_i} = \sum_k q(a_k)\frac{\partial \pi}{\partial h_i}(a_k)
$$
notice that given that the function $\pi(a_i)$ sums to one since its a probability and so its gradients sum to zero and therefore we can add a constant

$$
\frac{\partial E}{\partial h_i} = \sum_k (q(a_k) - B)\frac{\partial \pi}{\partial h_i}(a_k)
$$
with the proof given in [[Softmax]] the grafient of the softmax is:

$$
\frac{\partial E}{\partial h_i} = \sum_k (q(a_k) - B)\pi(a_k)(\delta_{ik} - \pi(a_i))
$$
notice that this is also an expectation since we have a sum weighted by the probabilities of the actions.

$$
\frac{\partial E}{\partial h_i} = E_k[(q(a_k) - B)(\delta_{ik} - \pi(a_i))]
$$

we can substitute q(a_k)  with R since E\[R/a_k\] is q(a_k) and we can choose the B as the average reward until t and get 

$$
\frac{\partial E}{\partial h_i} = E_k[(R - \bar R)(\delta_{ik} - \pi(a_i))]
$$
since the expected value of the formula 
$$
(R -\bar R)(\delta_{ik} - \pi(a_i))
$$
is the gradient of the expected return, our utility function, we can use the instance of the action taken and its reward and the average reward computed and update the vector of parameters h

$$
h_k = h_k + \alpha (R - \bar R)(\delta_{ik} - \pi(a_k))
$$
where k is any position and i is the position of the action taken.

tldr 
the preference of action k is changed thusly
if the reward is higher than average the preference of the action taken gets increased in accordance with how higher it is than average while all others are decreased to sum to one
and if the reward is lower than average the opposite happens.
