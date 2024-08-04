Agents follow a policy $\pi(a|s_n)$ that determines the probability of an action given a state.
the action causes a change in the environment which can change to a new state giving a reward  with a probability $P(s_{n+1},r|s_n, a)$.
#### Measures
of policy
gamma is the discount constant
$G_n = R_{n+1} + \gamma G_{n+1}$
of state
$v_{\pi}(s) = E_{\pi}(G_n|S_n = s)$ 
if actions and state
$q_{\pi}(s, a) = E_{\pi}(G_n|S_n = s, A = a)$ 
expected Gn for the state or the state and action considering in the future the agent followss $\pi$
$v_{\pi}(s) = \sum \limits_{a \in A} \pi(a, s) E(G_n|S = s)$
$G_n = R_{n+1} + \gamma G_{n+1}$
$E(G_n|S = s) = E(R_{n+1} + \gamma G_{n+1}|S = s)$
since its the expected this works
$E(G_n|S = s) = E(R_{n+1} + \gamma v_{\pi}(S_{n+1})|S = s)$
$E(G_n|S = s) = \sum \limits_{s_n, a} (P(s_{n+1},r|s_n, a))(r_{n+1} + \gamma v_{\pi}(s_{n+1}))$

therefore Bellman eq 1
$v_{\pi}(s) = \sum \limits_{a_0 \in A} \pi(a_0, s) \sum \limits_{s_n, a} (P(s_{n+1},r|s_n, a))(r_{n+1} + \gamma v_{\pi}(s_{n+1}))$
the value of the state s under policy $\pi$ is the expected next return plus the discounted value of the next state, the expectation is done over all the actions allowed in the policy and all the pairs of next state and reward allowed by the environment as responses to the policy

Bellman optimum eq
$v_{\pi*}(s) = argmax_{a \in A}(q_{\pi*}(s, a))$
every step is in the most increasing direction of q, greedy algo

