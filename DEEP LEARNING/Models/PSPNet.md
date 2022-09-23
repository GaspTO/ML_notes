Paper: [***Pyramid Scene Parsing Network***](https://arxiv.org/pdf/1612.01105.pdf)

# Overview
Introduces a new module called **pyramid pooling module** and uses it to create a new network called **Pyramid Scene Psing Network (PSPNet)**.

The idea is similar to the one proposed by the [[ParseNet|ParseNet]], which uses global pooling to complement the feature map.

The Pyramid Pooling Module is a simple idea based on the [[SPP-NET]] paper: it does a bunch of spatial pyramid pooling operations and then upsamples (through bilinear interpolation) and concatenates them.

The **PSPNet** they propose is based on a pre-trained [[ResNet|ResNet]] with [[Dilated Convolutions|dilated convolutions]] to extract the feature map. Then, a **pyramid pooling module** is applied to this map. Then a convolutional layer uses this new concatenated feature map to make a final prediction. This can be seen in the following figure:

![[Pasted image 20220121193103.png]]

To learn, they use an auxiliary classifier as seen in the figure.
![[Pasted image 20220121193323.png|300]]


___
#Vision #SemanticSegmentation #ConvolutionalNetwork #Architecture #Layer 
 