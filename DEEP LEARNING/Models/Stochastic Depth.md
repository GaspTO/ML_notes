Paper: ***Deep Networks with Stochastic Depth***

# Overview
This paper proposes deep networks with stochastic depth, a novel training algorithm that is based on the seemingly contradictory insight that ideally we would like to have a deep network during testing but a short network during training.

We resolve this conflict by creating deep [[ResNet|Residual Network]] architectures (with hundreds or even thousands of layers) with sufficient modeling capacity; however, during training we shorten the network significantly by randomly removing a substantial fraction of layers independently for each sample or mini-batch. The effect is a network with a small expected depth during training, but a large depth during testing. Although seemingly simple, this approach is surprisingly effective in practice.

The reduction in training time can be attributed to the shorter forward and backward propagation, so the training time no longer scales with the full depth, but the shorter expected depth of the network.

We also observe that similar to [[Dropout (to do)|Dropout]], training with stochastic depth acts as a regularizer, even in the presence of [[Batch Normalization (Inception-v2)]]

# Training
If a residual block is denoted by:
$$\begin{equation}
H_l = \text{ReLU}(f_l(H_{l-1};W_l) + H_{l-1})
\end{equation}$$

This paper disables blocks and not layers necessarily, so: Conv-BN-ReLU-Conv-BN. 
1. One way of doing this is by disabling the block number $l$ with $p_l$ probability, only allowing the residual connection to exist.
2. Another possibility is to disable the earlier layers with less probability than the last ones since, the earlier the layer, the more transversal its utility. The probability function can then be: $p_l = 1 - \frac{l}{L}(1-p_L)$, where $L$ is the total number of blocks.

**Discussion.** Implicit model ensemble. In addition to the predicted speedups, we also observe significantly lower testing errors in our experiments, in comparison with ResNets of constant depth. One explanation for our performance improvements is that training with stochastic depth can be viewed as training an ensemble of ResNets implicitly. Each of the L layers is either active or inactive, resulting in $2^L$ possible network combinations. For each training mini-batch one of the $2^L$ networks (with shared weights) is sampled and updated. During testing all networks are averaged using the approach in the next paragraph.

# Testing
During testing all blocks are enabled, but because during training each block were only active during a fraction $p_l$ of time, we  correct this during testing:
$$\begin{equation}
H_l = \text{ReLU}(p_l f_l(H_{l-1};W_l) + H_{l-1})
\end{equation}$$


# Experiments
1. **Results:**
![[Stochastic Depth f3.png]]
![[Stochastic Depth F4.png]]

2. **Time:**
Stochastic depth consistently gives a 25% speedup.
![[Stochastic Depth Table2.png|400]]

3. **1202-layer Resnet:**
The results are summarized in Fig. 4 (right) and Fig. 5. the ResNets with constant depth of 1202 layers yields a test error of 6.67%, which is worse than the 110-layer constant depth ResNet.
In contrast, if trained with stochastic depth, this extremely deep ResNet performs remarkably well.
![[Stochastic Depth f5.png]]

We want to highlight two trends: 
1) Comparing the two 1202-layer nets shows that training with stochastic depth leads to a 27% relative improvement
2) Comparing the two networks trained with stochastic depth shows that increasing the architecture from 110 layers to 1202 yields a further improvement on the previous record-low 5.25%, to a 4.91% test error without sign of overfitting, as shown in Fig. 4 (right)

___
#General #Vision #Layer #ResidualConnection 


