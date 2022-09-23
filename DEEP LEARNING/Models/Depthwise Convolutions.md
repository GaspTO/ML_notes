Paper: ***No paper***

**Depthwise Convolution** is a type of convolution where we apply a single convolutional filter for each input channel. In the regular 2D convolution performed over multiple input channels, the filter is as deep as the input and lets us freely mix channels to generate each element in the output. In contrast, depthwise convolutions keep each channel separate. To summarize the steps, we:

1.  Split the input and filter into channels.
2.  We convolve each input with the respective filter.
3.  We stack the convolved outputs together.

![[depthwiseconv.png]]