paper: ***Visualizing and Understanding Convolutional Networks***
[Here's a Talk about the paper by the author](https://www.youtube.com/watch?v=ghEmQSxT6tw)

[This version of the paper is a bit more explicit](https://link.springer.com/content/pdf/10.1007%2F978-3-319-10590-1_53.pdf).

This paper uses [[Deconvolutions (to do)|Deconvolutions]] to project features resulting from convolutional layers back towards the image space

Here's the pipeline:
![[Deconvolution layer.png]]

The filters and switches of the deconvolutions are shared with the convolutions. So, as opposed to the two previous papers (of the same author) about deconvolutions, this one does not have a spific way of learning filters. It learns them through the convolutional side as a normal network.

We can then see what our filters are learning in each layer
![[Visualization of features.png]]
Each 9 feature map matrix corresponds to one filter. They picked the top-9 strongest activations of the whole dataset and showed the true images on the right to show that there is a correlation and the filters are indeed learning specific things.

#Vision #Debug #Layer