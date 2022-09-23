Paper: [***A Simple Framework for Contrastive Learning of Visual Representations***](https://arxiv.org/pdf/2002.05709.pdf)

# Code
Has pre-trained weights on their [github](https://github.com/google-research/simclr)

# Overview
It's a deep study about certain properties of self-supervised learning. There are three major findings:
1. composition of data augmentations plays a critical role in defining effective predictive tasks.
2.  introducing a learnable nonlinear transformation between the representation and the contrastive loss substantially improves the quality of the learned representations
3.  contrastive learning benefits from larger batch sizes and more training steps compared to supervised learning.

Lastly, they use these findings to outperform many  previous self and semi-supervised learning  methods.

# SimCLR - Method
SimCLR learns representations by maximizing agreement between differently augmented views of the same data example via a contrastive loss in the latent space:
![[Pasted image 20220127154649.png|400]]

It has **4** major components:
1. **A stochastic data augmentation module** (In this work, we sequentially apply three simple augmentations: random cropping followed by resize back to the original size, random color distortions, and random Gaussian blur).
2. **A neural network base encoder $f$  that extracts representation vectors** from augmented data examples.
3. **A small neural network projection head $g$ that maps representations to the space where contrastive loss is applied.**
4. **A contrastive loss function defined for a contrastive prediction task.**


**Batch:** A batch of $N$ images is sampled and two transformations are applied to each, giving $2N$ data samples per batch.

**Loss:** 
$$\begin{equation}
-\log{
	\frac{\exp(\text{sim}(\mathbf{z_i},\mathbf{z_j})/\tau)}
	{\sum_{k=1}^{2N}\mathbb{1}_{[k \ne i]}\exp (\text{sim}(\mathbf{z_i},\mathbf{z_j})/\tau)}
}
\end{equation}$$

Where $\text{sim}$ is the cosine similarity  $\text{sim}(\mathbf{u},\mathbf{v}) = \frac{\mathbf{u}^\top \mathbf{v}}{||\mathbf{u}||\  ||\mathbf{v}||}$

The final loss is computed across all positive pairs, both (i, j) and (j, i), in a mini-batch.


Summary:
![[Pasted image 20220127162707.png|400]]



# Experiments
As default, after the pre-training talked about, they train the rest (mostly on imagenet) by attaching a linear layer to the $f$ function.

## Data Augmentation
### Composition
![[Pasted image 20220127163056.png|400]]
We observe that no single transformation suffices to learn good representations, even though the model can almost perfectly identify the positive pairs in the contrastive task. When composing augmentations, the contrastive prediction task becomes harder, but the quality of representation improves dramatically.

One composition of augmentations stands out: random cropping and random color distortion. We conjecture that one serious issue when using only random cropping as data augmentation is that most patches from an image share a similar color distribution.

### Contrastive learning needs stronger data augmentation than supervised learning
![[Pasted image 20220127163321.png|400]]


##  Architectures
### Parameters
Unsupervised constrative loss benefits more from enlarging models than supervised learning
![[Pasted image 20220127163242.png|400]]

### Head
The head used as default was a linear one, but the study actually finds out that non linear is better
![[Pasted image 20220127163515.png|400]]	


## Loss and Batch size
### Loss functions
NT-Xent is the one proposed in this article
![[Pasted image 20220127163900.png|400]]

Then they test the importance of the $l^2$ normalization (i.e. cosine similarity vs dot product) and temperature $\tau$ in their default NT-Xent loss
![[Pasted image 20220127164039.png|400]]

### Batch size
Contrastive learning benefits (more) from larger batch sizes and longer training
![[Pasted image 20220127163718.png|400]]


# Conclusions
## Disadvantages
- big batch size


___
#SelfSupervisedLearning #ContrastiveLearning #Vision 