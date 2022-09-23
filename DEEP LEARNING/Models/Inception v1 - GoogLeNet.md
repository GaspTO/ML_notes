- Paper: ***Going deeper with convolutions***

# Overview
**GoogLeNet** is a type of convolutional neural network based on the Inception architecture. . It introduced the new concept of inception block in CNN, whereby it incorporates multi-scale convolutional transformations using split, transform and merge idea.  Inception modules allow the network to choose between multiple convolutional filter sizes in each block. An Inception network stacks these modules on top of each other, with occasional max-pooling layers with stride 2 to halve the resolution of the grid. 

Furthermore, connection’s density was reduced by using global average pooling at the last layer, instead of using a fully connected layer

![[Inception Module.png]]

The dimension reduction inception module aggregates all the feature maps, dimension by dimension, and, by using less filters, it reduces the number of them while keeping the dimensions.

![[GoogLeNet.png]]
![[GoogLeNet 1.png]]


It also used intermediate classifiers. But the paper **Rethinking the Inception Architecture for Computer Vision**, aka inception-v3, concluded that this did not help with convergence, but that it held a slightly higher pleateau. Instead, they argue that the auxiliary classifiers act as regularizer. This is supported by the fact that the main classifier of the network performs better if the side branch is batch-normalized or has a dropout layer. This also gives a weak supporting evidence for the conjecture that batch normalization acts as a regularizer.


The main drawback of the GoogleNet was its heterogeneous topology that needs to be customized from module to module. Another limitation of GoogleNet was a representation bottleneck that drastically reduces the feature space in the next layer and thus sometimes may lead to loss of useful information.

___
#Vision #Architecture #Layer #ConvolutionalNetwork #Inception 
