### Convolutional layer

In the convolutional layer both the input and output are 3D arrays that can be thought of as vectors of matrices or images, if the first layer of the network is convolutional usually the vector of images contains the three matrices of red, green and blue pixel values, in this project since its just black or white it only contains one matrix but it still is ordered in a 3D array to keep a consistent structure. In the output of the first and in deeper convolutional layers the entries of the vector of matrices still resemble the original image but do not represent the colors but rather diferent features the network picks up.

The central operation in this layer is the convolution or crosscorrelation, which in the case of images takes in an image and a kernel(smaller 2D array) and outputs another image, the kernel is slide through the input image and the overlapping entries are multiplied together and summed, in a 2D analog of the dot product, and the result of that product is stored in the corresponding index of the output image

$$\textbf{v} = k*\textbf{u}$$

$$v_{ij} = \sum \limits_{(a,b) = (0,0)}^{(KShape)} (u_{(i-{\lfloor}\frac{L}{2}{\rfloor} + a,j-{\lfloor}\frac{L}{2}{\rfloor} + b)} \cdot k_{ab})$$

each image in the output vector of images is afected by every image in the input, and its a sum of the convolution of all the images with kernels specific for each input image and output image, the result is analog to a linear combination and a bias

$$
\textbf{y}_n = \sum \limits_{m} \textbf{k}_{nm}*\textbf{x}_m + \textbf{b}_n
$$

If we define an analog product $$(\cdot *)$$ that operates on two matrices of images or kernels and makes the convolution between the images and kernels and adds them together

$$
\textbf{y} =  l( \textbf{x},( \textbf{K}, \textbf{b})) = \textbf{K} (\cdot*) \textbf{x} + \textbf{b}
$$

$$\theta = ( \textbf{K} , \textbf{b})$$
___

index expansion of function $l$

$$y_{nij} =  \sum \limits_{m} [\textbf{k}_{nm}*\textbf{x}_m]_{ij} + b_{nij}
=\sum \limits_{m} (\sum \limits_{(a,b) = (0,0)}^{(KShape)} (x_{m,(i-{\lfloor}\frac{L}{2}{\rfloor} + a,j-{\lfloor}\frac{L}{2}{\rfloor} + b)} \cdot k_{nmab})) + b_{nij}
$$
___
$\frac{\partial E}{\partial x}$:

$$
\frac{\partial y_{nij}}{\partial x_{luv}} 
= \frac{\partial }{\partial x_{luv}}(\sum \limits_{m} (\sum \limits_{(a,b) = (0,0)}^{(KShape)} (x_{m,(i-{\lfloor}\frac{L}{2}{\rfloor} + a,j-{\lfloor}\frac{L}{2}{\rfloor} + b)} \cdot k_{nmab})) + b_{nij})
=\sum \limits_{m} (\sum \limits_{(a,b) = (0,0)}^{(KShape)} (\frac{\partial }{\partial x_{luv}}x_{m,(i-{\lfloor}\frac{L}{2}{\rfloor} + a,j-{\lfloor}\frac{L}{2}{\rfloor} + b)} \cdot k_{nmab}))
=\sum \limits_{m} (\sum \limits_{(a,b) = (0,0)}^{(KShape)} ( \delta_{ml} \delta_{u,(i-{\lfloor}\frac{L}{2}{\rfloor} + a)} \delta_{v,j-{\lfloor}\frac{L}{2}{\rfloor} + b)} k_{nmab}))
=\sum \limits_{(a,b) = (0,0)}^{(KShape)} (\delta_{u,(i-{\lfloor}\frac{L}{2}{\rfloor} + a)} \delta_{v,(j-{\lfloor}\frac{L}{2}{\rfloor} + b)} k_{nlab})
$$

$$
\frac{\partial y_{nij}}{\partial x_{luv}} 
=\sum \limits_{(a,b) = (0,0)}^{(KShape)} (\delta_{u,(i-{\lfloor}\frac{L}{2}{\rfloor} + a)} \delta_{v,(j-{\lfloor}\frac{L}{2}{\rfloor} + b)} k_{nlab})
$$

$U = (i-{\lfloor}\frac{L}{2}{\rfloor} + a)$

$V = (j-{\lfloor}\frac{L}{2}{\rfloor} + b)$

$a = U-i + {\lfloor}\frac{L}{2}{\rfloor}$

$b = V-j + {\lfloor}\frac{L}{2}{\rfloor}$

$$
\frac{\partial y_{nij}}{\partial x_{luv}} 
=\sum \limits_{(a,b) = (0,0)}^{(KShape)} (\delta_{u,U} \delta_{v,V} k_{nlab})
$$

$$
\frac{\partial y_{nij}}{\partial x_{lUV}} 
=k_{nlab}
$$

$$
\frac{\partial y_{nij}}{\partial x_{l,U,V}} 
=k_{n,l,(U-i + {\lfloor}\frac{L}{2}{\rfloor}),(V-j + {\lfloor}\frac{L}{2}{\rfloor})}
$$

$$
\frac{\partial E}{\partial x_{l,U,V}}
=\sum \limits_{n,i,j = (0,0,0)}^{shape(y)} (\frac{\partial E}{\partial y_{nij}}\frac{\partial y_{nij}}{\partial x_{l,U,V}} )
$$

$$
\frac{\partial E}{\partial x_{l,U,V}}
=\sum \limits_{n = 0}^{feats} (\sum \limits_{i,j = (0,0)}^{ImShape}\frac{\partial E}{\partial y_{nij}}\frac{\partial y_{nij}}{\partial x_{l,U,V}} )
$$

$$
\frac{\partial E}{\partial x_{l,U,V}}
=\sum \limits_{n = 0}^{feats} (\sum \limits_{i,j = (0,0)}^{ImShape}\frac{\partial E}{\partial y_{nij}}k_{n,l,(U-i + {\lfloor}\frac{L}{2}{\rfloor}),(V-j + {\lfloor}\frac{L}{2}{\rfloor})} )
$$

i and j are now convolutional indexes 90 degree rotated kernel

$$
\frac{\partial E}{\partial x_{l}}
=\sum \limits_{n = 0}^{feats} (\frac{\partial E}{\partial y_{n}} * k_{nl})
$$

$$
\frac{\partial E}{\partial \textbf x}
=\frac{\partial E}{\partial \textbf y} (\cdot * ) \textbf K
$$

___
$$y_{nij}
=\sum \limits_{m} (\sum \limits_{(a,b) = (0,0)}^{(KShape)} (x_{m,(i-{\lfloor}\frac{L}{2}{\rfloor} + a,j-{\lfloor}\frac{L}{2}{\rfloor} + b)} \cdot k_{nmab})) + b_{nij}
$$

$$
\frac{\partial y_{nij}}{\partial k_{\nu\mu uv}}
=\frac{\partial }{\partial k_{\nu\mu uv}}(
\sum \limits_{m} (\sum \limits_{(a,b) = (0,0)}^{(KShape)} (x_{m,(i-{\lfloor}\frac{L}{2}{\rfloor} + a,j-{\lfloor}\frac{L}{2}{\rfloor} + b)} \cdot k_{nmab})) + b_{nij}
)
$$

$$
\frac{\partial y_{nij}}{\partial k_{\nu\mu uv}}
=\sum \limits_{m} (\sum \limits_{(a,b) = (0,0)}^{(KShape)} (x_{m,(i-{\lfloor}\frac{L}{2}{\rfloor} + a,j-{\lfloor}\frac{L}{2}{\rfloor} + b)} \cdot \frac{\partial k_{nmab}}{\partial k_{\nu\mu uv}}))
$$

$$
\frac{\partial y_{nij}}{\partial k_{\nu\mu uv}}
=\sum \limits_{m} (\sum \limits_{(a,b) = (0,0)}^{(KShape)} (x_{m,(i-{\lfloor}\frac{L}{2}{\rfloor} + a,j-{\lfloor}\frac{L}{2}{\rfloor} + b)} \delta_{\nu n} \delta_{\mu m} \delta_{u a} \delta_{v b}))
$$

$$
\frac{\partial y_{nij}}{\partial k_{\nu\mu uv}}
=\delta_{\nu n} x_{\mu,(i-{\lfloor}\frac{L}{2}{\rfloor} + u,j-{\lfloor}\frac{L}{2}{\rfloor} + v)} 
$$

$$
\frac{\partial E}{\partial k_{\nu\mu uv}}
= \sum \limits_{nij = (0,0,0)}^{shape(y)}(\frac{\partial E}{\partial y_{nij}} \frac{\partial y_{nij}}{\partial k_{\nu\mu uv}})
$$

$$
\frac{\partial E}{\partial k_{\nu\mu uv}}
= \sum \limits_{nij = (0,0,0)}^{shape(y)}(\frac{\partial E}{\partial y_{nij}} \cdot \delta_{\nu n} \cdot x_{\mu,(i-{\lfloor}\frac{L}{2}{\rfloor} + u,j-{\lfloor}\frac{L}{2}{\rfloor} + v)})
$$

$$
\frac{\partial E}{\partial k_{\nu\mu uv}}
= \sum \limits_{ij = (0,0)}^{ImShape}(\frac{\partial E}{\partial y_{\nu ij}} \cdot x_{\mu,(i-{\lfloor}\frac{L}{2}{\rfloor} + u,j-{\lfloor}\frac{L}{2}{\rfloor} + v)})
$$

$$
\frac{\partial E}{\partial k_{\nu\mu uv}}
= \sum \limits_{ij = (0,0)}^{ImShape}(\frac{\partial E}{\partial y_{\nu ij}} \cdot x_{\mu,(i-{\lfloor}\frac{L}{2}{\rfloor} + u,j-{\lfloor}\frac{L}{2}{\rfloor} + v)})
$$

$$
\frac{\partial E}{\partial k_{\nu\mu}}
= \frac{\partial E}{\partial y_{\nu}} * x_{\mu}
$$

$$
\frac{\partial E}{\partial \textbf K}
= \frac{\partial E}{\partial \textbf y} (\otimes *) \textbf x
$$
___
$$
\frac{\partial E}{\partial \textbf b} = \frac{\partial E}{\partial \textbf y} 
$$
___
