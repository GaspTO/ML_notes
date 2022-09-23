Paper: [***ParseNet: Looking Wider To See Better***](https://arxiv.org/abs/1506.04579)

# Overview
The idea is to give global context to each local decision. The example they show is one animal being segmented as two different ones. When, if a big part of the animal is segmented as one, then it is more likely that the rest should be segmented as that one.


Theoretically, features from the top layers of a network have very large receptive fields. They argue that in practice, the empirical size of the receptive fields is much smaller, and is not enough to capture the global context. They do an empirical test where they put noise is certain parts of the image and see if the activations on the other side of the input image change. From their example, the effective receptive field at the last layer of this network barely covers $1/4$ of the entire image.

# Context Module
![[Pasted image 20220113232822.png]]

## Getting Global Context
To get global context, they use global average pooling to pool from the last layer or any other layer of interest.
The quality of semantic segmentation is greatly improved by adding the global feature to local feature map, either with **early fusion** or **late fusion** (next section).

## Merging Context
Once we get the global context feature, there are two general standard paradigms of using it with the local feature map.
1. First, the early fusion, where we unpool (replicate) global feature to the same size as of local feature map spatially and then concatenate them, and use the combined feature to learn the classifier.
2. The alternative approach, is late fusion, where each feature is used to learn its own classifier, followed by merging the two predictions into a single classification score. 
This early and late fusion are not ideas introduced in this work.
In their experiments show that both method works more or less the same if we normalize the feature properly for early fusion case.

## Normalization
They need to combine two (or more) feature vectors, which generally have different scale and norm. Naively concatenating features leads to poor performance as the ”larger” features dominate the ”smaller” ones. They find that by normalizing each individual feature first, and also learn to scale each differently, it makes the training more stable and improves performance.

They apply L2-norm and learn the scale parameter for each channel before using the feature for classification, which leads to more stable training.

Careful, however, because if the scaling is done so that the L2-norm is 1, the features become too small. Higher values (e.g. 10, 20...) are better for training.

we introduce a scaling parameter γi , for each channel, which scales the normalized value by $y_i = \gamma_i \hat{x}_i$, where $\hat{x} = \frac{x}{||x||^2}$. The number of extra parameters is equal to total number of channels, and are negligible and can be learned with backprogation. In fact, $γ_i = ||x||^2$, we could recover the L2 normalized feature, if that was optimal.


# Experiments
They use FCN as backbone. They use pool6 as global context. 
![[Pasted image 20220114010218.png]]

![[Pasted image 20220114010320.png]]

Pretty much: global context is good and nomalizing it is also good.



___
#Vision #SemanticSegmentation #ConvolutionalNetwork #Architecture 
 