Paper: [***Rethinking Atrous Convolution for Semantic Image Segmentation***](https://arxiv.org/pdf/1706.05587.pdf)


# Overview
This paper follows [[DeepLab-v2]] and [[DeepLab-v1]]. Unlike the previous ones, it does not use a dense CRF. It uses [[Dilated Convolutions|dillated convolutions]] or, as called in this article, *atrous convolutions*.  The whole paper is about how to use atrous convolutions to increase results.

An important note is that in this version of deep lab, unlike the previous ones, batch normalization is used, which might be responsible for a lot of the improvements.

To understand the use of atrous convolutions, see this example:
There is an input feature map with $100\times100$ dimensions and you use a maxpool  of stride 2, lowering the dimensions to $50\times50$ and then apply a kernel 7 filter, resulting in $44\times44$ dimensions. Lastly, you upsample it using a factor of 2, resulting in an $88\times88$ feature map.

Or, on the other hand you can simply use a atrous convolutions with rate 2 and a kernel of 7 (which gives 13 effective kernel size) which results in an $88\times88$ feature map. 

The good thing about using an atrous convolution and not a 13 sized kernel is that the parameters are not changed.

Here's, visually the example (this is from deeplab-v2):
![[Pasted image 20220126011239.png|400]]

This way, we don't need to first lower the image's resolution, which, even though we upsample the feature map after, it loses quality.


# How to use Atrous Convolutions
This paper explores two types of using atrous convs.
1. Laying out in Cascade.  They take the last block (the number fourth) of a ResNet,create several copies and arrange them in cascade. They use different atrous rates in each block.
![[Pasted image 20220126014624.png]]

2. Atrous Spatial Pyramid Pooling (ASPP) - proposed in deeplab-v2. Four parallel atrous convolutions with different atrous rates are applied on top of the feature map.  Then concatenate all the filters (they probably pad them before the convolutions) and then apply a $1\times1$ convolution.
![[Pasted image 20220126015336.png]]


#  Experiments
## Cascade
They start with the **Cascade mode**. 
### ResNet-50 and different output stride:
The output stride is just the proportion of the reduction between the input image and the output feature map.  They use a ResNet-50 with the 7 blocks previously mentioned.

![[Pasted image 20220126021150.png|400]]

### ResNet-100 and varying blocks:
More blocks, the better

![[Pasted image 20220126021418.png|400]]

### ResNet-100 and varying the atrous rate 
Rate in the block 4, 5, 6 and 7.
The final atrous rate for the convolutional layer is equal to the multiplication of the unit rate and the corresponding rate. For example, when output stride = 16 and Multi Grid = (1, 2, 4), the three convolutions will have rates = 2 · (1, 2, 4) = (2, 4, 8) in the block4, respectively.

![[Pasted image 20220126021648.png|400]]
 
### Training vs Inference
The proposed model is trained with output stride = 16, and then during inference we apply output stride = 8 to get more detailed feature map
![[Pasted image 20220126024036.png|400]]

## Atrous Spatial Pyramid Pooling
###  Atrous rate and ASPP rate
we experiment with the effect of incorporating multi-grid in block4 and image-level features to the improved ASPP module. We first fix ASP P = (6, 12, 18) (i.e., employ rates = (6, 12, 18) for the three parallel 3 × 3 convolution branches), and vary the multigrid value. Employing Multi Grid = (1, 2, 1) is better than Multi Grid = (1, 1, 1), while further improvement is attained by adopting Multi Grid = (1, 2, 4) in the context of ASP P = (6, 12, 18) (cf ., the ‘block4’ column in Tab. 3). If we additionally employ another parallel branch with rate = 24 for longer range context, the performance drops slightly by 0.12%. On the other hand, augmenting the ASPP module with image-level feature is effective, reaching the final performance of 77.21%.
![[Pasted image 20220126024058.png|400]]

### Training vs Inference
Similarly, we apply output stride = 8 during inference once the model is trained.
![[Pasted image 20220126024305.png|400]]

#  Conclusions
Both our best cascaded model and ASPP model already outperform DeepLabv2. The improvement mainly comes from including and fine-tuning batch normalization parameters in the proposed models and having a better way to encode multi-scale context.






