Paper:[***Multi-scale context aggregation by dilated convolutions***](https://arxiv.org/pdf/1511.07122.pdf)


# Overview
![[dilatedconvolutions.gif|200]]

The definition of the original convolution:
$$\begin{equation}
(F * k)(\mathbf{p}) = \sum_{\mathbf{s+t=p}}F(\mathbf{s})k(\mathbf{t})
\end{equation}$$
where $F$ is the map and $k$ is the kernel.

The definition of the dilated convolution:
$$\begin{equation}
(F * k)(\mathbf{p}) = \sum_{\mathbf{s+lt=p}}F(\mathbf{s})k(\mathbf{t})
\end{equation}$$
where $l$ defines the dilation. 
Visit the definition in the deeplearning book to gain more insight.

If we have three dilated conv layers in a row, the first with $l=1$, the second with $l=2$ and the last with $l=3$ we have a receptive field that increases exponentially:
![[Pasted image 20220113010936.png]]
The green is the receptive field and the red is the margins of the filter.


# Context Module
They propose a module called *context module*. The basic context module has 7 layers that apply 3×3 convolutions with different dilation factors. The dilations are 1, 1, 2, 4, 8, 16, and 1. Each of these convolutions is followed by a pointwise truncation max(·, 0). A final layer performs 1×1×C convolutions and produces the output of the module.

## Initialization
Their early attempts to train the context module failed. Apparentely it only works in the appropriate initialization:
$$\begin{equation}
k^b(\mathbf{t},a) = 1_[\mathbf{t}=0]1_[a=b]
\end{equation}$$
where $a$ is the index of the input feature map and $b$ is the index of the output map. This is a form of identity initialization introduced in this paper originally for RNNs] in *"A Simple Way to Initialize Recurrent Networks of Rectified Linear Units".*

There are 2 types of context module used: basic and large, they only differ in the number of output maps per layer:
![[Pasted image 20220113013843.png]]

# Experiments
1. They train a 'front-end' based on VGG using dilated convolutions:

They adapted the VGG-16 network (Simonyan & Zisserman, 2015) for dense prediction and removed the last two pooling and striding layers. Specifically, each of these pooling and striding layers was removed and convolutions in all subsequent layers were dilated by a factor of 2 for each pooling layer that was ablated. Thus convolutions in the final layers, which follow both ablated pooling layers, are dilated by a factor of 4. This enables initialization with the parameters of the original classification network, but produces higher-resolution output. The front-end module takes padded images as input and produces feature maps at resolution 64×64.

![[Pasted image 20220113013044.png]]


2. Using the context module.

We begin by plugging each of the two context modules (Basic and Large) into the front end. Since the receptive field of the context network is 67×67, we pad the input feature maps by a buffer of width 33.


Comparing standard, basic and large context block:
![[Pasted image 20220113013914.png]]

Comparing the large (the best) context block to the PASCAL VOC 2012 competition.
![[Pasted image 20220113013927.png]]



___
#Vision #SemanticSegmentation #ConvolutionalNetwork #Layer 