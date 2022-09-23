Paper: [***Barlow Twins: Self-Supervised Learning via Redundancy Reduction***](https://arxiv.org/pdf/2103.03230.pdf)


# Overview
This is a self-supervised learning technique that differs from the others in the loss function.

It achieved really good results.

It does not need asymmetric mechanisms to work.

![[Pasted image 20220205220324.png]]


# Method
They use a siamese network.  
The algorithm is:
1. Sample a batch of images, $X$.
2. Apply data augmentation to $X$ by sampling from the possible transformations $\mathcal{T}$, getting $Y_A$ and $Y_B$.
3. They are then fed to the network, $f_\theta$, producing the representation batches, $Z_A$ and $Z_B$, which, for notation purposes, are assumed to be centered at 0 over the batch.
4. Apply loss:
$$\begin{equation}
\mathcal{L}_{BT} = \underbrace{\sum_i (1-\mathcal{C}_{ii})^2}_\text{invariance term} + \lambda \underbrace{\sum_i \sum_{j \neq i} C_{ij}^2}_\text{redundacy reduction term}
\end{equation}$$

where $\lambda$ is a positive constant trading off the importance of the first and second terms of the loss, and where $\mathcal{C}$ is the cross-correlation matrix computed between the outputs of the two identical networks along the batch dimension:

$$\begin{equation}
\mathcal{C}_{ij} = \frac{\sum_b z_{b,i}^A + z_{b,j}^B}
{\sqrt{\sum_b(z_{b,i}^A)^2} \sqrt{\sum_b(z_{b,j}^B)^2}}
\end{equation}$$

where $b$ indexes batch samples and $i$, $j$ index the vector dimension of the networks’ outputs. $\mathcal{C}$ is a square matrix with size the dimensionality of the network’s output, and with values comprised between $-1$ (i.e. perfect anti-correlation) and $1$ (i.e. perfect correlation).

Intuitively, the ***invariance term*** of the objective, by trying to equate the diagonal elements of the cross-correlation matrix to 1, makes the embedding invariant to the distortions applied.

The ***redundancy reduction term***, by trying to equate the off-diagonal elements of the cross-correlation matrix to 0, de-correlates the different vector components of the embedding. This de-correlation reduces the redundancy between output units, so that the output units contain non-redundant information about the sample.

## Information theory
More formally, BARLOW TWINS’s objective function can be understood through the lens of information theory, and specifically as an instantiation of the Information Bottleneck (IB) objective.

Applied to self-supervised learning, the IB objective consists in finding a representation that conserves as much information about the sample as possible while being the least possible informative about the specific distortions applied to that sample.

(More on the appendix A).

## Implementation
- Resnet-50 backbone (without final classification layer), followed by a projector network.
- The projector network has 3 linear layers, each with 8192 units.
- The first two layers of the projector are followed by a BN ReLU.
- We call the output of the encoder the ’representations’ and the output of the projector the ’embeddings’.
- The representations are used for downstream tasks and the embeddings are fed to the loss function of BARLOW TWINS.

# Experiments

## Linear classifier on ImageNet
Training linear classifier
![[Pasted image 20220205230034.png|300]]

Training linear classifier with limited data
![[Pasted image 20220205230245.png|300]]

## Ablations
### Loss function
![[Pasted image 20220205230401.png|300]]

### Batch size
![[Pasted image 20220205230417.png|300]]

### Removing augmentations
![[Pasted image 20220205230644.png|300]]

### Breaking symmetry
![[Pasted image 20220205230737.png|300]]

### Project network depth & width
![[Pasted image 20220205230444.png|300]]

###  Sensitivity $\lambda$
![[Pasted image 20220205230822.png|300]]







___
#SelfSupervisedLearning  #ContrastiveLearning  