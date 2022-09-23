Paper: ***Aggregated Residual Transformations for Deep Neural Networks***

# Overview
This paper introduces a new dimension in network blocks called *cardinality* and this is the number of paths an input *travels* in parallel. The [[Inception v1 - GoogLeNet|Inception module]] is an example of this, it has multiple paths and then converges all of them with a concatenation. The ResNets can be thought as having two paths.

The paper shows that by changing the cardinality of the blocks, even by maintaining the overall complexity, the network can improve performance.

The mentality in the ResNext is the same as in the [[VGG]]/[[ResNet|ResNet]], meaning that it is about adding depth to the network by using equal blocks. Also, each time when the spatial map is downsampled by a factor of 2, the width of the blocks is multiplied by a factor of 2. 



The operations designed are based on: *splitting*, *transforming* and *aggregating*.  We can see a feed forward layer, $\sum_{i=1}^{D}w_ix_i$, through this lenses:
- **Splitting:** the vector $x$ is sliced as a low-dimensional embedding, and in the above, it is a single-dimension subspace $x_i$.
- **Transforming:** the low-dimensional representation is transformed, and in the above, it is simply scaled: $w_ix_i$.
- **Aggregating:** the transformations in all embeddings are aggregated by $\sum_{i=1}^{D}$

We can now generalize this but to an aggregated function, $\mathcal{T}_i$:
$$\begin{equation}
\mathcal{F(\mathbf{x})} = \sum_{i=1}^{C}\mathcal{T}_i(\mathbf{x})
\end{equation}$$
where $C$ is the cardinality. 

In this work, all $\mathcal{T}_i$ have the same topology.

We can add a residual connection
$$\begin{equation}
\mathbf{y} = \mathbf{x} + \sum_{i=1}^{C}\mathcal{T}_i(\mathbf{x})
\end{equation}$$

Here's an example of a resblock and a resnext block that has pretty much the same parameters, but a bigger cardinality ($C=2$):
![[ResNet vs ResNext blocks.png|400]]


The article also shows the equivalence the resnext blocks with summation and concatenation and group convolutions:
![[Resnext equivalent blocks.png]]


There is a natural trade-off between cardinality and bottleneck width (the number of channels):
![[table2 resnext.png|400]]


# Experiments
## Cardinality vs. Width
![[table3 resnext.png|400]]
Even the training error diminuishes, showing that it is not regularization but learning strong features.

## Increasing Cardinality vs. Deeper/Wider
![[table4 resnext.png|400]]
Increasing complexity through cardinality seems better than increasing width or depth.

___
#Vision  #ConvolutionalNetwork  #ResidualConnection #Layer


