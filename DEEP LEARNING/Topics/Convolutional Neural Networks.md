***This is a general page about convolutional neural networks***

# Useful Resources
[Google's basic CNN intro](https://developers.google.com/machine-learning/practica/image-classification/convolutional-neural-networks)

# Convolutional Layers
A _convolution_ extracts tiles of the input feature map, and applies filters to them to compute new features, producing an output feature map, or _convolved feature_ (which may have a different size and depth than the input feature map). Convolutions are defined by two parameters:

-   **Size of the tiles that are extracted** (typically 3x3 or 5x5 pixels).
-   **The depth of the output feature map**, which corresponds to the number of filters that are applied.
-   **width**, the number of channels in a layer.
-   **Output stride** explains the ratio of the input image size to the output feature map size. It defines how much signal decimation the input vector suffers as it passes the network. For an output stride of 16, an image size of $224\times224\times3$ outputs a feature vector with 16 times smaller dimensions. That is $14\times14$
- **Resolution** the higher, the bigger the amount of "pixels" in the input image.

During a convolution, the filters (matrices the same size as the tile size) effectively slide over the input feature map's grid horizontally and vertically, one pixel at a time, extracting each corresponding tile (see Figure 3).!


![Convolution_overview|300](convolution_overview.gif)

In practice, as written in the DeepLearning book, the operation used is called **cross-correlation:**
$$\begin{equation}
S(i,j) = (I \ast K)(i,j) = \sum_{m} \sum_{n} I(i+m,j+n) K(m,n)
\end{equation}$$

In the output map, one position of the map is connected to many positions (pixels) in the original image. All those pixels are the **receptive field** of that output point.



# Equivariance and Invariance
In the case of convolution, the particular form of parameter sharing causes the
layer to have a property called equivariance to translation. To say a function is
equivariant means that if the input changes, the output changes in the same way. Speciﬁcally, a function f (x) is equivariant to a function g if f(g(x)) = g(f (x)). - Deep Learning,  Goodfellow 

# FIlter importance
The early filters are useful of most classes, but they become more class specific in deeper layers (How transferable are features in deep neural networks?).


# Capture multi-scale context
From Paper: ***Rethinking Atrous Convolution for Semantic Image Segmentation:***
![[Pasted image 20220125183437.png]]

___
#General #ConvolutionalNetwork 