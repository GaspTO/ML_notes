Paper: ***A ConvNet for the 2020s***

# Overview
ConvNeXt essentially takes a ResNet and gradually "modernizes" it to discover components that contribute to performance gains. ConvNeXt applies several tricks like larger kernels, layer norm, fewer activation functions, separate downsampling layers to name a few. ConvNeXt performs 87.8% on ImageNet-1k, outperforms Swin Transformers on COCO detection and ADE20K segmentation, and offers a simple and efficient implementation. These results show that hybrid models are promising and that different components can still be optimized further and composed more effectively to improve the overall model on a wide range of vision tasks.

![[Pasted image 20220117193608.png|400]]
 
 

# Modernizing a ConvNet
The roadmap of our exploration is as follows. Our starting point is a ResNet-50 model. First, we train it with similar training techniques used to train vision Transformers and obtain much improved results compared to the original ResNet-50. This will be our baseline. We then study a series of design decisions which we summarized as:
1. Macro Design
2. ResNext
3. Inverted Bottleneck
4. Large Kernel Size
5. Various layer-wise micro design
 
 Summary of results:
 ![[Pasted image 20220117195022.png|300]]
 
 Performance is measured on imageNet.

## Training
Before looking at the design, we'll look at training.  They take a ResNet-50 and train it similarly to vision transformers. The training is similar to the Swin Transformer and DeiT.  . The training is extended to 300 epochs from the original 90 epochs for ResNets.  AdamW optimizer is used. Data augmentation techniques such as Mixup, Cutmix, RandAugment, Random Erasing, and regularization schemes including Stochastic Depth and Label Smoothing.

**This enhanced training of ResNet-50 from 76.1% to 78.8%, implying that a significant portion of the performance difference between traditional ConvNets and vision Transformers may be due to the training techniques.**

They use this training recipe in the following design decisions.

## 1. Macro Design
- **Changing stage compute ratio.** Changes each stage of ResNet-50 from (3,4,6,3) to (3,3,9,3), which follows the computational distribution of the Swin Trasnformers of 1:1:3:1.  **Improvement from 78.8% to 79.4%**

- **Changing stem to “Patchify”.** 	 In Vision Transformers a more aggressive “patchify” strategy is used as the stem cell, which corresponds to a large kernel size (e.g. kernel size = 14 or 16) and non-overlapping convolution. They will use the “patchify stem” (4×4 non-overlapping convolution) in the network. **Improvement from 79.4% to 79.5%**


## 2. ResNeXt-ify
Adopt the ResNext core idea. The core component is grouped convolution, where the convolutional filters are separated into different groups. More precisely, ResNeXt employs grouped convolution for the 3×3 conv layer in a bottleneck block. 
In our case we use depthwise convolution, a special case of grouped convolution where the number of groups equals the number of channels.
We note that depthwise convolution is similar to the weighted sum operation in self-attention, which operates on a per-channel basis, i.e., only mixing information in the spatial dimension. **Improvement to 80.5%**.


## 3. Inverted Bottleneck
One important design in every Transformer block is that it creates an inverted bottleneck, i.e., the hidden dimension of the MLP block is four times wider than the input dimension

The difference between normal bottleneck vs residual bottleneck:
![[Pasted image 20220118020540.png]]

Using the same idea of inverted bottleneck popularized by MobileNet. **The bottle neck improved from 80.5% to 80.6%**.
Fig (a) and (b): 
![[Pasted image 20220118005703.png]]


## 4. Large Kernel Size
One of the most distinguishing aspects of vision Transformers is their non-local self-attention, which enables each layer to have a global receptive field.

In order to use large kernels, they move the depthwise convolution to the first layer that only has 96 filters instead of 384 as seen in (c) of the previous figure. **This temporarily decreased perfomance to 79.9%**.

They tried several kernels, but the maximum was 7x7 depthwise conv, **with an improvement to 80.6%**.


## 5. Micro Design
This section is a bunch of small decisions that matter.

- **Replacing ReLU with GELU**. **It doesn't yield any improvement**, but it was something that worked in NLP transformers
- **Fewer Activation Functions**. The transformer head only has one sigmoid after some layers. The newblock will only have an activation/GeLU in the last layer:
![[Pasted image 20220118021550.png]]
**this improves performance to 81.3%**.
- **Fewer Normalization Layers**.  Transformer blocks usually have fewer normalization layers as well. Here we remove two BatchNorm (BN) layers, leaving only one BN layer before the conv 1 × 1 layers - **this improves performance to 81.4%**.
- **Substituting BN with LN**. **Performance improves to 81.5%**.
- **Separate downsampling layers**. In ResNet, the spatial downsampling is achieved by the residual block at the start of each stage, while in Swin Transformers, a separate downsampling layer is added between stages. They explore a similar strategy in which we use 2×2 conv layers with stride 2 for spatial downsampling. This modification leads to diverged training. Further investigation shows that, adding normalization layers wherever spatial resolution is changed can help stablize training. These include several LN layers also used in Swin Transformers: one before each downsampling layer, one after the stem, and one after the final global average pooling. **Perforamance improves to 82%**.


 
 
 







