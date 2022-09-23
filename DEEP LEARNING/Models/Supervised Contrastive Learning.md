Paper: [***Supervised Contrastive Learning***](https://arxiv.org/pdf/2004.11362.pdf)

# Overview
This paper uses the ideas of self-supervised learning and applies them to supervised learning. The idea is to use a contrastive loss that approximates the representations of images belonging to the same label.

![[Pasted image 20220208012149.png]]

**Contributions:**
1. Contrastive loss function that allows for multiple positives per anchor, good for fully supervised setting.
2. Their loss provides consistent boosts in top-1 accuracy for a number of datasets. It is also more robust to natural corruptions.
3. Showing the gradient of their loss function encourages learning from hard positives and hard negatives.
4. Their loss is less sensitive than cross-entropy to a range of hyper-parameters.
	
# Method
## Three components
1. *Data Augmentation modul*e, $\text{Aug}(\cdot)$. For each input sample, $x$, we generate two random augmentations, $\tilde{x} = \text{Aug}(x)$.
2. *Encoder Network*, $\text{Enc}(\cdot)$, which maps $x$ to a representation vector, $r = \text{Enc}(x) \in \mathcal{R}^{D_E}$. Both augmented samples are separately input to the same encoder, resulting in a pair of representation vectors. r is normalized to the unit hyper-sphere.
3. *Projection Network*, $\text{Proj}(\cdot)$, which maps $r$ to a vector $z = \text{Proj}(r) \in \mathcal{R}^{D_P}$ . We instantiate $\text{Proj}(\cdot)$ as either a multi-layer perceptron  with a single hidden layer of size 2048 and output vector of size DP = 128 or just a single linear layer of size DP = 128.. We discard $\text{Proj}(\cot)$ at the end of contrastive training. As a result, our inference-time models contain exactly the same number of parameters as a cross-entropy model using the same encoder, $\text{Enc}(\cdot)$.


## Algorithm
- Given an input batch of data, apply data augmentation twice to obtain two copies of the batch.
- Both copies are forward propagated through the encoder network to obtain a 2048-dimensional normalized embedding.
- During training, this representation is further propagated through a projection network that is discarded at inference time.
- The supervised contrastive loss is computed on the outputs of the projection network.
- To use the trained model for classification, we train a linear classifier on top of the frozen representations using a cross-entropy loss.

## Loss
### traditional loss
Many self-supervised losses are of the type:
$$\begin{equation}
\mathcal{L}^\text{self} = \sum_{i\in I} \mathcal{L}_i^\text{self} = - 
\sum_{i\in I} \log \frac{\exp (\mathbf{z}_i \cdot \mathbf{z}_{j(i)}/\tau)}{\sum_{a\in A(i)} \exp(\mathbf{z}_i \cdot \mathbf{z}_a)/\tau}
\end{equation}$$

Here, $\mathbf{z}_l = \text{Proj}(\text{Enc}(\mathbf{\tilde{x}}_t))$ and $\tau$ is a numeric constant, and $A(i) \equiv I\{i\}$. The index $i$ is called the anchor, index $j(i)$ is called the positive, and the other $2(N − 1)$ indices are called *negatives*. Note that for each anchor $i$, there is $1$ positive pair and $2N − 2$ negative pairs.

### supervised contrastive losses
The previous (traditional) loss has a problem, which is not considering that in a batch there might be more than one sample belonging to the same label.  The paper proposed two extensions to the previous loss.

1. $$\begin{equation}
\mathcal{L}_{out}^{sup} = \sum_{i \in I} \frac{-1}{|P(i)|} \sum_{p \in P(i)} \log \frac{\exp (\mathbf{z}_i \cdot \mathbf{z}_{j(i)}/\tau)}{\sum_{a\in A(i)} \exp(\mathbf{z}_i \cdot \mathbf{z}_a)/\tau}
\end{equation}$$

2.  $$\begin{equation}
\mathcal{L}_{in}^{sup} = \sum_{i \in I} - \log \bigg[
\frac{1}{|P(i)|} \sum_{p \in P(i)} \frac{\exp(\mathbf{z}_i \cdot\mathbf{z}_p/\tau)}{\sum_{a\in A(i)} \exp(\mathbf{z}_i \cdot \mathbf{z}_a)/\tau}
\end{equation}$$

**Both of these have the following properties:**
- Generalization to an arbitrary number of positives.
- Contrastive power increases with more negatives.
- Intrinsic ability to perform hard positive/negative mining.

**They are, however, not equivalent.**
![[Pasted image 20220208030002.png]]

(They do put a theoretical/mathematical explaination about the two - check the paper ).


# Experiments
They experimented with three commonly used encoder architectures: ResNet-50, ResNet-101, and ResNet-200.

Four different implementations of the $\text{Aug}(\cdot)$ data augmentation module: AutoAugment,  RandAugment,  SimAugment, and Stacked RandAugment.
## Classification
Using ResNet-50.
They achieve a new state of the art accuracy of $78.7\%$ on ResNet-50 with [[AutoAugment]]
![[Pasted image 20220208030317.png]]


## Data Augmentations
![[Pasted image 20220208030903.png]]






___
#Vision   #ContrastiveLearning #SupervisedLearning