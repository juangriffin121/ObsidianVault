# Entropy Experiment

This project intends to show the relation between the physical definition of entropy and the information theory definition and understanding the second law of thermodynamics in terms of the information entropy.

## Physical definition

There are two definitions of entropy that come from two realms of physics:
- Thermodynamics: 
	$$dS = \frac{\delta Q_{rev}}{T}$$
	And thus the change in entropy of the system after going from state A to state B is
	$$S_2 - S_1 = \int^{B}_{A} \frac{\delta Q_{rev}}{T}$$
- Statistical Mechanics: 
	proportional to the log of the number of micro-states the system could be in
	$$S = -k_B \ln (\Omega)$$
	Which is a particular case of the definition below
	$$S = -k_B E[\ln(p)]$$



## Information theory definition
Average value of a measure of "surprise"
Average number of questions to ask to get to a point of the distribution
Average number of bits needed to communicate an instance of the distribution in a perfect encoding
$$S = E[\ln (p)]$$

## Shannon and Boltzmann entropy

Similar, for discrete possible states like in quantum mechanics, the definition applies exactly, the information entropy of the distribution is proportional to the Boltzmann entropy of the system in that state.
In the case of a continuum of possible micro-states, such as position and velocities of atoms in an ideal gas, there are two ways of understanding it, with probability density functions(PDF), or with bins and histograms, in both of those cases the concept of number of possible micro-states no longer applies, but the same formula, with an integral instead of a sum works just as well for a measure of how concentrated the distribution is around its likely states.
The interpretation of the probabilities can be two things:
- A simple histogram, ie creating a discrete grid of bins where you consider groups of states to be in the same bin and the probability of one bin is the probability that picking a random point from all of the points will be in that particular bin.
- Continuum PDF will represent lack of knowledge of the exact position of the atoms but certain regions of state-space are more likely than others.

##  But why does it increase?
Up until now, we've been talking about a static probability distribution, which is where information entropy is usually used like when using it for losses on neural nets and such, but in those cases its not used to analyze a changing probability distribution according to some dynamic laws.

Using the continuum PDF interpretation of the probabilities, a dynamic system with stochastic behavior can be understood as the limit of a continuum state-space Markov Chain
$$f(s, t) = P_{t}(S=s)$$
$$f(s, t+dt) =  \int_{s' \in \textit{S}} P(S=s|S'=s')f(s', t)ds'$$
$$f(s, t+dt) =  \int_{s' \in \textit{S}} \phi _{dt}(s,s')f(s', t)ds'$$ $$\frac{\partial f}{\partial t}(s, t) = \lim_{dt \rightarrow 0} \frac {\int_{s' \in \textit{S}} \phi _{dt}(s,s')f(s', t)ds' - f(s, t)}{dt}$$
$$  \frac{\partial f}{\partial t}(s, t)= \lim_{dt \rightarrow 0} \frac {\int_{s' \in \textit{S}} \phi _{dt}(s,s')f(s', t)ds' - \int_{s' \in \textit{S}}\delta(s - s')f(s', t)ds'}{dt}$$

$$ \frac{\partial f}{\partial t}(s, t) = \lim_{dt \rightarrow 0} \frac {\int_{s' \in \textit{S}} ( \phi _{dt}(s,s') - \delta(s - s'))f(s', t)ds'}{dt}$$
$$ \frac{\partial f}{\partial t}(s, t) = \lim_{dt \rightarrow 0} \int_{s' \in \textit{S}}\frac {\phi _{dt}(s,s') - \delta(s - s')}{dt}f(s', t)ds'$$
$$ \frac{\partial f}{\partial t}(s, t) = \int_{s' \in \textit{S}} G(s, s')f(s', t)ds'$$

The function $G(s, s')$ defines the dynamical system