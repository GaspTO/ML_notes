 Paper: ***Densely Connected Convolutional Networks***

# Overview
The idea is that each layer connects to all the next layers using residual connections. The total residual connections then becomes $\frac{l(l-1)}{2}$.

In practice, not the whole network becomes dense, but dense blocks are defined.
![[Dense Block.png|300]]
![[Densenet.png]]

There are 5 points in this paper:
1.	**Dense connectivity.** the $\text{l}^th$ layer receives the feature-maps of all preceding layers, $\mathbf{x}_0$, . . . , $\mathbf{x}_{l-1}$, as input. 
$$\begin{equation}	
\mathbf{x}_l = H_l([\mathbf{x}_0,...,\mathbf{x}_l])
\end{equation}$$
where $H_l$ is the function of the layer and the $[\mathbf{x}_0,...,\mathbf{x}_l]$ refers to the concatenation of the feature-maps produced in layers $0$, . . . , $l−1$.

2. **Composite function.** $H_l$ is defined as a composition of three operations: **batch normalization**, **rectified linear unit** and a $\mathbf{3\times3}$ **convolution**.

3. **Pooling Layers.** The concatenation operation used in the previous equation is not viable when the size of feature-maps changes. However, an essential part of convolutional networks is down-sampling layers that change the size of feature-maps.  To facilitate down-sampling in our architecture we divide the network into multiple densely connected **dense blocks**. The transition layers used in our experiments consist of a batch normalization layer and an 1×1 convolutional layer followed by a 2×2 average pooling layer.

4. **Growth rate.** If each function $H_l$  produces k featuremaps, it follows that the  $l^{th}$ layer has $k_0 +k\times(l−1)$ input feature-maps, where $k_0$ is the number of channels in the input layer. An important difference between DenseNet and existing network architectures is that DenseNet can have very narrow layers, e.g., $k = 12$. $k$ is called the growth rate.  One can view the feature-maps as the global state of the network. The growth rate regulates how much new information each layer contributes to the global state. The global state, once written, can be accessed from everywhere within the network and, unlike in traditional network architectures, there is no need to replicate it from layer to layer.

5. **Bottleneck layers.** Although each layer only produces k output feature-maps, it typically has many more inputs. It has been noted in other architectures that a 1×1 convolution can be introduced as bottleneck layer before each 3×3 convolution to reduce the number of input feature-maps, and thus to improve computational efficiency.

1. **Compression.** To further improve model compactness, we can reduce the number of feature-maps at transition layers. If a dense block contains m feature-maps, we let the following transition layer generate $[\theta m]$ output featuremaps, where $0 < \theta \leq 1$ is referred to as the compression factor.


___
#Vision #Architecture #ConvolutionalNetwork #ResidualConnection