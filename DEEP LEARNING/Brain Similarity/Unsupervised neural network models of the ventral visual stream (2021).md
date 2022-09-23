## Task
we compared each unsupervised neural network described in the previous section to neural data from macaque V1, V4, and IT cortex.

## Unsupervised methods
![[Pasted image 20220413161306.png]]


## Evaluation method
 Specifically, we fit a regularized linear regression model from network activations of each unsupervised model to neural responses collected from array electrophysiology experiments in the macaque ventral visual pathway.
 
 We also compare the unsupervised networks both to the supervised network, which represents a previously known positive control, and to an untrained model, which represents an objective function-independent architecture-only baseline. Given that network architecture remains fixed across all objective functions, the outcome isolates the impact of choice of objective function on neural predictivity.

## Data
Comparison to area V1 was made using neural data collected by Cadena et al. (19), while comparison to areas V4 and IT was made using data collected by Majaj et al. (3, 52). Although these two datasets were collected with different experimental designs, they have both previously been used to evaluate (supervised) deep neural network models, yielding consistent results.
The image sets on which V4 and IT neural responses are predicted are quite distinct both in type and content from the training data for the unsupervised networks (ImageNet), representing a strong generalization test for the representations


## Results
**Overall, the unsupervised methods that had higher task transfer performance predicted neural responses substantially better than less-performant unsupervised methods.**

* In the first cortical visual area V1, all unsupervised methods were significantly better than the untrained baseline at predicting neural responses, although none were statistically better from the category-supervised model on this metric
*  In contrast, in intermediate cortical area V4, only a subset of methods achieved parity with the supervised model in predictions of responses. (Interestingly, the deep autoencoder was not better than the untrained model on this metric and both were widely separated from the other trained models.) 
* For IT cortex at the top of the ventral pathway, only the best-performing contrastive embedding methods achieved neural prediction parity with supervised models. Among these methods, the local aggregation model, which has recently been shown to achieve state-of-the-art unsupervised visual recognition transfer performance, also achieved the best neural predictivity.
* Similarly, good neural predictivity is also achieved by two other contrastive embedding methods, instance recognition (IR) and SimCLR. More specifically, IR shows comparable V1, V4, and IT predictivity to the supervised mode

The next figure shows thr neural predictivity for each brain area from all network layers, for several representative unsupervised networks, including AutoEncoder, colorization, and local aggregation. 


![[Pasted image 20220413160457.png]]



we also investigated which layers of DCNNs best matched cortical brain areas


