Paper: ***Rethinking the Inception Architecture for Computer Vision***

# Overview
## Design Principles:
1. Avoid representational bottlenecks, especially early in the network. The representational size should slowly and progressively decreased.
2. Higher dimensional representations are easier to process locally within a network. Increasing the activations per tile in a convolutional network allows for more disentangled features. The resulting networks will train faster. (I think that by: more activations per tile, they mean the number of activations to result in the current tile).
3. Spatial aggregation can be done over lower dimensional embeddings without much or any loss in representational power. (They refer to pooling). We hypothesize that the reason for that is the strong correlation between adjacent unit results in much less loss of information during dimension reduction.
4. Balance the width and depth of the network. Optimal performance of the network can be reached by balancing the number of filters per stage and the depth of the network. The optimal improvement for a constant amount of computation can be reached if both are increased in parallel.

## Factorize Large Filter Convolutions
There are two ways to reduce a large filter to smaller ones. The idea is to replace a filter layer by several layers of reduced filters.  Convolutions with larger spatial filters (e.g. 5 × 5 or 7 × 7) tend to be disproportionally expensive in terms of computation. For example, a 5 × 5 convolution with n filters over a grid with m filters is 25/9 = 2.78 times more computationally expensive than a 3 × 3 convolution with the same number of filters. Two ways:

1. The first is to reduce using the same filter dimension. Here's an example of how to replace a 5x5 convolution filter by two layers of 3x3. The number of parameters is less in the two layer replacement
	![[Replace 5x5 convolution.png]]
	
	![[inception-v3 inception module without 5x5 conv.png]]

2. It is also possible to use asymmetrical convolutions. For instance, a $1\times n$ convolution.   For example:
  ![[1xn and nx1 convolution.png]]
  This factorization did not hold well when applied in earlier layers in the network.
  

## Pooling then conv   VS conv then pooling?
Convolutional networks use some pooling operation to decrease the grid size of the feature maps. In order to avoid a representational bottleneck, before applying maximum or average pooling the activation dimension of the network filters is expanded. If we do pooling before convolutions, that saves computation, but creates a bottleneck immediately after the pooling, breaking the principle number 1.  They use the following module:

![[Cheaper inception module in inception v3.png]]

![[Reduce grid size..png]]



## INCEPTION - v3, Architecture
- The traditional inception 7 × 7 convolution was transformed into three 3 × 3 convolutions.
- For the Inception part of the network, we have 3 traditional inception modules at the 35×35 with 288 filters each. This is reduced to a 17 × 17 grid with 768 filters using the grid reduction technique
- This is is followed by 5 instances of the factorized inception modules as depicted in figure 5. This is reduced to a 8 × 8 × 1280 grid with the grid reduction technique depicted in figure 10.
- At the coarsest 8 × 8 level, we have two Inception modules as depicted in figure 6, with a concatenated output filter bank size of 2048 for each tile.

![[Inception-v3 table architecture.png]]


However, we have observed that the quality of the network is relatively stable to variations as long as the principles from Section 2 are observed. Although our network is 42 layers deep, our computation cost is only about 2.5 higher than that of GoogLeNet and it is still much more efficient than VGGNet


## Training
instead of using a one-hot vector target, they gave some probability to each class to avoid the model becoming too confident.
$$\begin{equation}
	q(k|x) = (1-\epsilon)\delta_{k,y} + \epsilon u(k)
\end{equation}$$
where $\delta_{k,y}$ is 1 when the class $k$ is the same as the true class $y$ and 0 otherwise. In their experiments, the best was when $u(k) = 1/K$. 

This technique was called ***label-smoothing regularization***, or **LSR**

___
#Vision #Architecture #ConvolutionalNetwork #Inception 
