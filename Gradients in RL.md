---
id: Gradients in RL
aliases: []
tags: []
---

My own deductions of how it works, it might be wrong
We define a policy parameterized by a preference vector
$\vec \phi$  that follows the functional form [[Softmax]]
$$\pi(a_i | s_k) = \frac{mask(i,k)(exp(\phi_i))}{\sum_j mask(j,k)(exp(\phi_j))}$$

$$\frac{\partial \pi(a_i | s_k)}{\partial \phi_m} = \frac{\partial}{\partial \phi_m}\frac{mask(i,k)(exp(\phi_i))}{\sum_j mask(j,k)(exp(\phi_j))}$$
$$\frac{\partial \pi(a_i | s_k)}{\partial \phi_m} =  mask(i,k)\frac{exp(\phi_i)}{\sum_j mask(j,k)(exp(\phi_j))}$$
$$
\frac {\partial}{\partial \phi_m} (e^{\phi_i})(\sum \limits_{j}mask(j,k)e^{\phi_j})^{-1} =
\frac {\partial e^{\phi_i}}{\partial \phi_m}(\sum \limits_{j}mask(j,k)e^{\phi_j})^{-1} +
	e^{\phi_i} \frac {\partial}{\partial \phi_m} (\sum \limits_{j}mask(j,k)e^{\phi_j})^{-1}
$$
$$
\frac {\partial e^{\phi_i}}{\partial \phi_m}(\sum \limits_{j}mask(j,k)e^{\phi_j})^{-1} +
	e^{\phi_i} \frac {\partial}{\partial \phi_m} (\sum \limits_{j}e^{\phi_j})^{-1} =
\delta_m^i e^{\phi_i} (\sum \limits_{j}mask(j,k)e^{\phi_j})^{-1} +
	e^{\phi_i} (-1)
	(\sum \limits_{j}mask(j,k)e^{\phi_j})^{-2}\frac {\partial e^{\phi_m}}{\partial \phi_m} =
\delta_m^i\frac{ e^{\phi_i}} {(\sum \limits_{j}mask(j,k)e^{\phi_j})} -
	\frac{e^{\phi_i} e^{\phi_m}}{ (\sum \limits_{j}mask(j,k)e^{\phi_j})^2} 
$$

$$
mask(i,k)(\delta_m^i\frac{ e^{\phi_i}} {\sum \limits_{j}mask(j,k)e^{\phi_j}} -
	\frac{e^{\phi_i} e^{\phi_m}}{ (\sum \limits_{j}mask(j,k)e^{\phi_j})^2}) =
 \delta_m^i \pi(a_i,s_k) - \pi(a_i,s_k) \pi(a_m,s_k) = \pi(a_i,s_k)(\delta_m^i - \pi(a_m,s_k))
$$

$$\frac{\partial \pi(a_i | s_k)}{\partial \phi_m} = \pi(a_i,s_k)(\delta_m^i - \pi(a_m,s_k))$$
where $\phi_i$ is the preference for action i, this vector should actually depend on the current state.

it should have certain properties:
- It should theoretically have $-\infty$ preference for unavailable actions in the state.
- Simple functional dependence on state

the simplest form is having a OHE vector of state and multiply it by a matrix $\theta_{ij}$  such that
$$\phi_i = \sum_j \theta_{ij} s_j$$
since s is a OHE vector this is equivalent to $\theta_{ij}$ being the preference of action i given state j and then we can mask the unavailable actions

$mask(i,k)$ = {$1$ if $i$ available in state $k$, $0$ c.c.} 


$v_{\pi}(s) = E_{\pi}(G_n|S_n = s)$ 
$G_n = R_{n+1} + \gamma G_{n+1}$
following current policy $\pi$ and doing many episodes we get an approximation if $q_{\pi}$ 
we'll step in the gradient direction of the following function
$$argmax_{\pi} [\sum_k E_{\pi}(G_n|S_n = s_k)] = argmax_{\pi}\sum_k v_{\pi}(s_k)$$
it can also be weighted by state with a $\mu(s)$
$$v_{\pi}(s_k) = \sum_j \sum \limits_{s_{n+1}} \pi(a_j, s_k)  P(s_{n+1},r|s_k, a)(r_{n+1} + \gamma v_{\pi}(s_{n+1})) = \sum_j \pi(a_j,s_k)q_{\pi}(s_k,a_j)$$

$$\frac{\partial v_{\pi}}{\partial \theta_{i j}} (s_k) = \sum_l \frac{\partial \pi(a_l, s_k)}{\partial \theta_{i j}} q_{\pi}(s_k,a_l)$$
because of the deduction in [[Softmax]]
$$\frac{\partial \pi(a_l, s_k)}{\partial \phi_m} = \pi(a_l, s_k)(\delta_m^l - \pi(a_m, s_k))$$
$$\frac{\partial \phi_m}{\theta_{ij}} = \delta_i^m s_j$$
$$\frac{\partial v_{\pi}}{\partial \theta_{i j}} (s_k) = \sum_l \sum_m \pi(a_l, s_k)(\delta_m^l - \pi(a_m, s_k)) \delta_i^m s_j  q_{\pi}(s_k,a_l)$$

$$\frac{\partial v_{\pi}}{\partial \theta_{i j}} (s_k) = 
\sum_l \sum_m (\pi(a_l, s_k)\delta_m^l - \pi(a_l, s_k)\pi(a_m, s_k)) \delta_i^m s_j  q_{\pi}(s_k,a_l)=
\sum_l \sum_m (\pi(a_l, s_k)\delta_m^l \delta_i^m s_j  q_{\pi}(s_k,a_l) -
	\pi(a_l, s_k)\pi(a_m, s_k) \delta_i^m s_j  q_{\pi}(s_k,a_l))=
\sum_l (\pi(a_l, s_k)\delta_i^l s_j  q_{\pi}(s_k,a_l) -
	\pi(a_l, s_k)\pi(a_i, s_k) s_j  q_{\pi}(s_k,a_l))=
\sum_l (\delta_i^l  -
	\pi(a_i, s_k))
	\pi(a_l, s_k) s_j  q_{\pi}(s_k,a_l)=
$$

### Update rule
we aproximate q(s_k,a_i) simulating multiple episodes with current policy, and using that data we can compute the change, then with the new policy new $q_\pi$ we can do multiple SGD steps once we have a new action value function computed, how many before the approximation of close policy stops being true idk, must read
also the sum over k can be done just with one or a few states perhaps with probability given by $\mu$
$$
\theta_{ij} \leftarrow \theta_{ij} + \lambda \sum_k \mu(s_k) \sum_l 
 q_{\pi}(s_k,a_l) \pi(a_l, s_k) (\delta_i^l- \pi(a_i, s_k)) s_j 
$$
if the formula for pi uses a NN the formula follows easy
next thing to learn, value and action function aproximation with gradients

one other simplification to the update rule which would allow us to perform only one simulation per update is use a rougher estimate of q(a,s) and that is simply the return Gt gotten when encountering the pair in the current trajectory  since the expected value of the found return Gt following the current policy after taking action a in state s is the definition of q(a,s) the update rule still follows the [[Stochastic Gradient Descent Theorem?]] 

mu goes away since we are sampling from the trajectory
$$
\theta_{ij} \leftarrow \theta_{ij} + \lambda  
  g_t \pi(a_t, s_t) (\delta_{a_i}^{a_t}- \pi(a_i, s_t)) s_j
$$
divide by $\pi$
$$
\theta_{ij} \leftarrow \theta_{ij} + \lambda  
  g_t (\delta_{a_i}^{a_t}- \pi(a_i, s_t)) s_j
$$

$$
\theta_{ij} \leftarrow \theta_{ij} + \lambda  
\nabla_{ij} log\pi(a_t,s_t) g_t
$$
$$
\vec \theta \leftarrow \vec\theta + \lambda  
\vec \nabla log\pi(a_t,s_t) g_t
$$
see [[Policy Gradient Theorem]]
