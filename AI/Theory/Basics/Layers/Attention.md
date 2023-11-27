The attention algorithm is a contiuous (with embedded vectors instead of strings) version of the search for queries in a dstabase or python dictionarys, in the discrete case a query string is inputed and check for matches in the keys of the database, when a match is found returns the value asosiated with the matching key.
In the embedded version of words instead of using absolute matches we use closeness in the embedded vector space meassured by the dot product of their embedded vectors, so to "check for matches" the algorithm performs a dot product between the query vector and the set of key vectors, and since we dont use an absolute match, instead of returning only one value, we generate a probability distribuiton with the [[Softmax]] function over the key value pairs according to the dot products of the keys with the query and return a linear combination or a weighted average of the values with that distribution.
When theres more than one query we have a matrix Q as the set of query vectors, the matrix reoresentation of the operation is written below.
$Attention(Q,K,V) = \frac {Softmax(Q K^{T})}{\sqrt {d_{model}}}V$
The unscaled algorithm performs poorly on large values of dmodel "We suspect that for large values of
dk, the dot products grow large in magnitde, pushing the [[Softmax]] function into regions where it has
extremely small gradients. To counteract this effect, we scale the dot products by $\frac{1}{\sqrt {d_{k}}}$." (Paper: Attention is all you need)

#### Gradient descent
$$
Y = Attention(Q,K,V)
$$
$$
X = QK^T
$$
$$
Z = SoftMax(X)/d
$$
---
$$Y_{ij} = \sum \limits_k Z_{ik}V_{kj}$$
$$
\frac {\partial Y}{\partial Z}: \frac {\partial Y_{ij}}{\partial Z_{uv}} =  \frac {\partial }{\partial Z_{uv}}\sum \limits_k Z_{ik}V_{kj} = \sum \limits_k \frac {\partial Z_{ik}}{\partial Z_{uv}} V_{kj} = \sum \limits_k \delta_u^i \delta_v^k V_{kj} = \delta_u^i V_{vj}
$$
$$\frac {\partial Y}{\partial Z}=I \otimes V$$
$$\frac {\partial Y}{\partial V}: \frac {\partial Y_{ij}}{\partial V_{uv}} =  \frac {\partial }{\partial V_{uv}} \sum \limits_k Z_{ik}V_{kj} = \sum \limits_k Z_{ik}\frac {\partial V_{kj}}{\partial V_{uv}} = \sum \limits_k Z_{ik} \delta_u^k \delta_v^j = Z_{iu}\delta_v^j$$

$$\frac {\partial Y}{\partial V}=Z \otimes I$$
$Z_{ik}$ shares i with $Y_{ij}$ while $V_{kj}$ shares j, the derivative is a delta that forces the shared indexes to be zero, times the other matrix in the multiplication.

----

For the SoftMax derivative, $X_{ij} = \vec Q_i \cdot \vec K_j$  for every query i the SoftMax is applied to the set of dot products with every key
$$
Z_{ij} = \frac{e^{X_{ij}}}{\sum \limits_{k}e^{Xik}} 
$$
$$\vec Z_i = SoftMax(\vec X_i)$$
$$\frac {\partial Z_{ij}}{\partial X_{uv}} = 0, u \not = i$$
$$
\frac {\partial Z_{ij}}{\partial  X_{iv}} = Z_{ij}(\delta_v^j - Z_{iv})
$$
---
$$
\frac {\partial X}{\partial Q}: \frac {\partial X_{ij}}{\partial Q_{uv}} =  \frac {\partial }{\partial Q_{uv}}\sum \limits_k Q_{ik}K^T_{kj} = \sum \limits_k \frac {\partial Q_{ik}}{\partial Q_{uv}} K^T_{kj} = \sum \limits_k \delta_u^i \delta_v^k K^T_{kj} = \delta_u^i K^T_{vj}
$$

$$
\frac {\partial X}{\partial K}: \frac {\partial X_{ij}}{\partial K_{uv}} =  \frac {\partial }{\partial K_{uv}} \sum \limits_k Q_{ik}K^T_{kj} = \sum \limits_k Q_{ik}\frac {\partial K^T_{kj}}{\partial K_{uv}} = \sum \limits_k Q_{ik}\frac {\partial K_{jk}}{\partial K_{uv}} = \sum \limits_k Q_{ik} \delta_u^j \delta_v^k = Q_{iv}\delta_u^j
$$
