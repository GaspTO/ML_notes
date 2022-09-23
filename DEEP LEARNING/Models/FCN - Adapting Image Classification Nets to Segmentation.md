Paper:[***Fully Convolutional Networks for Semantic Segmentation***](https://arxiv.org/pdf/1411.4038.pdf)

# Overview
This paper is old (2015) and showed how you could take networks like AlexNet, VGG and GoogLeNet and modify them to segmentation challenges. The strategies proposed are good enough to not have to train these networks from scratch, but mainly fine-tune.

Their key insight was to build fully convolutional networks, without even one fully connected layer.



# Adapt to Segmentation
## Fully connected to Fully convolutional
The fully connected layers have as input and output fixed dimensions. To change this, we can think of the fully connected layers as a kernel with the same dimensions as the fixed input size. In this way, if the input increased, we just reuse these connections that are now thought as a kernel and movie through the much larger input.
![[Pasted image 20220112143403.png|450]]
Furthermore, while the resulting maps are equivalent to the evaluation of the original net on particular input patches, the computation is highly amortized over the overlapping regions of those patches. 


## Reduce downsappling
The typical convnets downsample the size of the output maps (by a factor of $f$, by the same logic as before we can see a classifier that receives an image which has $224 \times 224$ can be seen as being downsampled by a factor of $224$).

1. **shift-and-stitch trick:** The first way this can be done is by shifting the input  (by left and top padding) $x$ pixels to the right and $y$ pixels down. (Suggested in the OverFeat paper).

2. Alternatively, wWe can always modify the strides and filters of the layers to reduce downsampling. Higher strides, higher downsamplings. If the stride of a layer is $s$, we can reduce it to $1$. Obviously, this changes the computation. However, convolving the next filter with the upsampled output does not produce the same result as the shift and stick trick, because the original filter only sees a reduced portion of its (now upsampled) input. We need to change the filter. And we can do that by zeroing some parts of it. 
$$ \begin{equation}
\Pr\{\mathrm{NC}|\alpha\}= \left\{
\begin{array}{ll}
      f_{i/s,j/s} & \text{if } s \text{ divides both } i \text{ and } j\\
      0 & \text{otherwise}
\end{array} 
\right. 
\end{equation}$$
Reproducing the full net output of the trick involves repeating this filter enlargement layerby-layer until all subsampling is removed.

Simply decreasing subsampling within a net is a tradeoff: the filters see finer information, but have smaller receptive fields and take longer to compute. We have seen that the shift-and-stitch trick is another kind of tradeoff: the output is made denser without decreasing the receptive field sizes of the filters, but the filters are prohibited from accessing information at a finer scale than their original design.

## Upsampling
If we can't reduce downsampling, we can always upsample. 

1. One way of doing it by using *backward convolutions* (aka [[Deconvolutions (to do)| deconvolutions]]). Upsampling is therefore done in-network. The deconvolution filter in such a layer need not be fixed (e.g., to bilinear upsampling), but can be learned. A stack of deconvolution layers and activation functions can even learn a nonlinear upsampling.
2. You can also use a simpler deconvolution like bilinear upsampling, that computes each output $y_ij$ from the nearest four inputs by a linear map that depends only on the relative positions of the input and output cells.

## Patchwise vs Fully convolutional training
Explaining this part according to [stackoveflow](https://stats.stackexchange.com/questions/266075/what-is-the-difference-between-patch-wise-training-and-fully-convolutional-train):
*'Basically, fully convolutional training takes the whole MxM image and produces outputs for all subimages in a **single ConvNet forward pass**. Patchwise training explicitly crops out the subimages and produces outputs for each subimage in **independent forward passes**. Therefore, fully convolutional training is usually substantially faster than patchwise training. 
[...]
while this is quite fast, it restricts your training sampling process compared to patchwise training: You are forced to make a lot of updates **on the same image** (actually, all possible updates for all subimages) during one step of your training. That's why they write that fully convolutional training is only identical to patchwise training, if each receptive field (aka subimage) of an image is contained in a training batch of the patchwise training procedure (for patchwise training, you also could have two of ten possible subimages from image A, three of eight possible subimages from image B, etc. in one batch). Then, they argue that by not using all outputs during fully convolutional training, you get closer to patchwise training again (since you are not making all possible updates for all subimages of an image in a single training step). However, you waste some of the computation. Also, in Section 4.4/Figure 5, they describe that making all possible updates works just fine and there is no need to ignore some outputs.'


# Architectures
## Adapting previous architectures
They picked [[AlexNet| AlexNet]],[[Inception v1 - GoogLeNet|GoogLeNet]] and [[VGG|VGG]]. They decapitate each net by discarding the final classifier layer, and convert all fully connected layers to convolutions. We append a 1 × 1 convolution with channel dimension 21 to predict scores for each of the PASCAL classes (including background) at each of the coarse output locations, followed by a deconvolution layer to bilinearly upsample the coarse outputs to pixel-dense outputs. The results:
![[Pasted image 20220112155527.png|400]]

The training was by fine-tuning and not triaining completely end-to-end (and even then it takes days).



## New Architecture
The problem with adapting from a previous architecture is that, by the time you deconvolve, you already lost much of the information.

They define a new FCN (fully convolution network):
![[Pasted image 20220112172332.png|]]

The reasoning to read the following diagram is as follows. There are 5 layers with different sizes from image to pool5 which is the maximum reduced (as it would by a classifier). There are 3 networks:
1. One, FCN-32 (VGG-16), that simply gets to this one pixel map and deconvolutes it to a 32 times augmntation.
2. Second, FCN-16, adds a pool4 with another map that results from deconvoluting the pool5 2 times. Then deconvolute that summed map 16 times.
3. Third, FCN-8, continues on the previous network, but instead of deconvoluting 16 times, it deconvolutes 2 times and adds pool3. Then it deconvolutes 8 times.
Results:
![[Pasted image 20220112232748.png|400]]


# Experiments
**Pascal VOC:**
![[Pasted image 20220112232951.png|400]]

**NYUDv2:**
![[Pasted image 20220112233111.png|400]]

**SIFT:**
![[Pasted image 20220112233133.png|400]]



___
#Vision #SemanticSegmentation #ConvolutionalNetwork #Architecture 
 






