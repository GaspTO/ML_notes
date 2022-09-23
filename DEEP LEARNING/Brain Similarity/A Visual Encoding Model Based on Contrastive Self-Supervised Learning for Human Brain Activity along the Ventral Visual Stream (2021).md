# Overview
this paper uses a self-supervised technique called SimCLR-v2 to train a network and see how similar it becomes with the visual cortex.

## Data
The data is five subjects’ brain fMRI activity while they were viewing natural color images of the ImageNet. The training set contained 1200 images from 150 representative categories (eight images per category), while the validation set included 50 images from 50 different categories (one image per category).


## Method
They train with the self-supervised approach and the supervised approach.
After training, they will map a linear classifier to try to predict the regions of interest of that fmri (the voxel, which is a 3D pixel) based on the features extracted from the networks.

The prediction ability of different visual encoding models for distinct visual regions can be displayed by comparing the correlation.

Next figure shows the overview of the proposed method.

![[Pasted image 20220420135642.png]]

## Models
They use a ResNet-50.

We extracted features from 18 different layers for each input sample, including the outputs of the first convolution layer, 16 bottlenecks, and the final average pooling layer. Each layer of the feature was used to construct the encoding model.

They train the ResNet-50 with self-supervised learning, supervised learning and they also fine-tune the self-supervised network with 1%, 10% and 100% of the imageNet data



## Result Analysis
1. The first measure is the (Pearson) correlation between the linear prediction between the supervised and self-supervised in the validated set
2. RSA



# Results
![[Pasted image 20220420141314.png]]


![[Pasted image 20220420141339.png]]



![[Pasted image 20220420141411.png]]

