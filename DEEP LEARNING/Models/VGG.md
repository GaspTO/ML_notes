Paper: [***Very Deep Convolutional Networks for Large-Scale Image Recognition***](https://arxiv.org/pdf/1409.1556.pdf)

# Overview
It takes sort of alexnet and varies sizes, using smaller window sizes in the convolutions.

VGG replaced the 11×11 and 5×5 flters with a stack of 3×3 flters layer and experimentally demonstrated that concurrent placement of small size (3×3) flters could induce the efect of the large size flter (5×5 and 7×7). The use of the small size flters provides an additional beneft of low computational complexity by reducing the number of parameters. These fndings set a new trend in research to work with smaller size flters in CNN.

The main limitation associated with VGG was the use of 138 million parameters, which make it computationally expensive and difficult to deploy it on low resource systems.

Lost to GoogleNet in the 2014-ILSVRC, but was noticed due to its simplicity.

Different variations:
![[VGG configurations.png|500]]


It uses [[Dropout (to do)|dropout]] to train.

___
#Vision #Architecture #ConvolutionalNetwork 



