Paper: ***Squeeze-and-Excitation Networks***

# Overview
This paper introduces a Squeeze-and-Excitation block that can be plugged into any convolutional layer.

The idea of this block is to multiply each channel between [0,1].
![[Pasted image 20220913010155.png]]

If $\mathbf{V} = [\mathbf{v}_1,...,\mathbf{v}_C]$ is a set of learned filters, where the $c^\text{th}$ filter is denoted by $\mathbf{v}_C$.

The **squeeze** part is done by creating a summary vector, $\mathbf{z}$, of each channel. This work uses average pooling:
$$\begin{equation}
z_c = \frac{1}{H \times W} \sum_{i=1}^{H} \sum_{j=1}^{W} u_c(i,j)
\end{equation}$$


The **excitation** part is done by creating a gating value in $[0,1]$ for each channel, using the previous statistics:
$$\begin{equation}
\mathbf{s} = \sigma (\mathbf{W}_2 \delta (\mathbf{W}_1 \mathbf{z}))
\end{equation}$$

$\mathbf{W}_1 \in \mathcal{R}^{\frac{C}{r} \times C}$ and $\mathbf{W}_2 \in \mathcal{R}^{C \times \frac{C}{r}}$ , $r$ is the reduction parameter that regulates the parameters in the block.

This statistics is nothing but a gating function that is now multiplied per channel
$$\begin{equation}
\mathbf{\hat{x}} = s_c \mathbf{u}_c
\end{equation}$$

**Discussion:** The excitation operator maps the input specific descriptor z to a set of channel weights. In this regard, SE blocks intrinsically introduce dynamics conditioned on the input, which can be regarded as a selfattention function on channels whose relationships are not confined to the local receptive field the convolutional filters are responsive to.

# Instantiations
The SE block can be integrated into standard architectures such as VGGNet by insertion after the non-linearity following each convolution. Moreover, the flexibility of the SE block means that it can be directly applied to transformations beyond standard convolutions.
![[SE inception module comparison.png|400]] 
![[SE inception module comparison with residual connections.png|400]]

![[SE blocks.png]]

# Experiments
to finish...
**Squeeze**
One of the experiments (7.1) looks at the effect of the squeeze and removes it and the excitation mechanism, replacing them by a 1x1 convolution outputting the same number of channels. This means the convolution cam re-map the feature maps, but, for each 1x1 pixel, it does not use global information.  This had a significant influence in the model, worsening it. The conclusion is that merging global information is useful.

**Excitation**


___
#General #Vision  #ConvolutionalNetwork #Layer 


