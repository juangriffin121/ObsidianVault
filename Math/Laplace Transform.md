Functional transform that takes in a function f in time domain and returns a function of frequency-decay domain, if the real component of the s complex input of the transformed function is 0 then the function of the complex component is the [[fourier transform]].
$$L\{f\}(s) = \int \limits_0^{\infty}f(t)e^{-st}$$
L is a linear operator
Poles(infinities) of the frequency function represent the behavior of the time function.
$$L\{e^{at}\}(s) = \frac{1}{s - a}$$

$$L\{\frac{df}{dt}\} = sL\{f\} - f(0)$$

L applied on linear differential equations results in a rational(ratio of polynomials of s).

$$ODE: \sum \limits_k^{order} \alpha_k \frac{d^kf}{dt^k} = 0$$
$$L\{ODE\}: \sum \limits_k^{order} \alpha_k (s^kL\{f\} - \sum \limits_j^{k-1}s^j \frac {d^jf}{dt^j}(0)) = 0$$
The convolution in time domain turns into product in frequency domain
$$L\{f*g\} = L\{f\}L\{g\}$$