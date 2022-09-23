Paper: [***Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition***](https://arxiv.org/pdf/1406.4729.pdf)

# Overview
This paper does not an architecture per say. This paper introduces a layer called spatial pyramid pooling layer, which accepts a variable size input so that any network that uses it can have a variable size input.
![[spatial pyramid pooling layer.png|400]]


This layer is a pooling layer, like max pooling. The idea is to define the output size and then the pooling window and stride is adapted according to the input size of the layer.  

The output size is formed by several spatial bins. These several usually go for less to more complex, therefore the name Pyramid Pooling.

For a input a x a, let's say the specific spatial bin in n x n, the window size is given by $\text{ceiling}(\frac{a}{n})$ and the stride is given by $\text{floor}(\frac{a}{n})$.

The idea of this paper was to replace the last pooling layer in popular architectures by the pyramid pooling layer. 

The one that worked the best is the Overfeat-7 architecture with this layer.

Something else interesting about this is that, since this layer allows the network to take variable input size, we can train the network by using the same images, but using different sizes as a form of image augmentation.

They also tested with multiple views.

___
#Vision #Architecture #ConvolutionalNetwork