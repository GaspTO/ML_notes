Paper: [***Exploring Simple Siamese Representation Learning***](https://arxiv.org/pdf/2011.10566.pdf)


# Overview
1) This papers take the simplicity of the [[Siamese Architecture|siamese nets]] and even removes  successfully what were before considered important parts: (i) negative sample pairs, (ii) large batches, (iii) momentum encoders.  
2) It shows in a mathematical, but not complicated way, why their method works; which is very insightful for self-supervised learning.

![[Pasted image 20220203181436.png|300]]

They show that, by not allowing the gradient to pass through one side of the siamese network they can obtain good results.

It removes asymmetry from both the siamese architecture and the way it is trained (with the stop-gradient function).

# Method
Just like in the siamese networks, the network, $f$, is replicated in two sides and two views of the sample image, $x$, goes through each channel.  The network $f$ is some other network's backbone (like a ResNet-50) plus some part that is a projection head.

In this method, we always try to maximize the similarity since we don't use negative pair examples. 

The difference is that, to one side, a prediction head *h* is added (as seen in the first Fig.), which projects the result of the network to some space. For two different views of the same image, $x_1$ and $x_2$, one side is given by $p_1 = h(f(\mathbf{x_1}))$  and the other by $z_2 = f(x_2)$.

These two's similarity can be given by, for example, the cosine similarity
$$\begin{equation}
\mathcal{D}(p_1,z_2) = - \frac{p_1}{||p_1||_2} \cdot \frac{z_2}{||z_2||_2}
\end{equation}$$

where $||\cdot||_2$ is the $l_2$ norm.

## Loss
The loss is symmetric, based on the previous similarity function.
$$\begin{equation}
\mathcal{L} = \frac{1}{2} \mathcal{D}(p_1,z_2) +  \frac{1}{2} \mathcal{D}(p_2,z_1)
\end{equation}$$

## Stopgrad
An important component for their method  is the **stopgrad** operation, that stops the gradient from flowing backwards.  The previous equation can then be written as
$$\begin{equation}
\mathcal{L} = \frac{1}{2} \mathcal{D}(p_1,\text{stopgrad}(z_2)) +  \frac{1}{2} \mathcal{D}(p_2,\text{stopgrad}(z_1))
\end{equation}$$

So, the only side that receives gradients is the one with the head $h$.



# Theoretical Hypothesis
## Formulation without predictor
The hypothesis is that **SimSiam** is an implementation of an Expectation-Maximization (EM) like algorithm. It implicitly involves two sets of variables, and solves two underlying sub-problems. The presence of stop-gradient is the consequence of introducing the extra set of variables. 

We consider a loss function of the following form:
$$\begin{equation}
\mathcal{L}(\theta,\eta) = \mathbb{E}_{x,\mathcal{T}}\bigg[ ||\mathcal{F}_\theta(\mathcal{T}(x))-\eta_x||^2_2 \bigg]
\end{equation}$$
$\mathcal{F}$ is a network parameterized by $\theta$. $\mathcal{T}$ is the augmentation. $x$ is an image. The expectation $\mathbb{E}[\cdot]$ is over the distribution of images and augmentations. We do not consider the predictor yet.

$\eta$ is another set of variables proportional to the number of images. Intuitively, $\eta_x$ is the representation of the image $x$ and is not necessarily the output of a network

**The optimization problem is therefore:**
$$\begin{equation}
\min_{\theta,\eta} \mathcal{L}(\theta,\eta)
\end{equation}$$

This formulation is analogous to k-means clustering. The variable $\theta$ is analogous to the clustering centers: it is the learnable parameters of an encoder. The variable ηx is analogous to the assignment vector of the sample $x$ (a one-hot vector in k-means): it is the representation of $x$.

Formally, we can alternate between solving these two subproblems:
$$\begin{equation}
\theta^t \leftarrow \arg \min_\theta \mathcal{L}(\theta,\eta^{t-1})
\end{equation} \tag{7}$$
$$\begin{equation}
\eta^t \leftarrow \arg \min_\eta \mathcal{L}(\theta^t,\eta)
\end{equation} \tag{8} $$

### Solving for θ
One can use SGD to solve the sub-problem 7.
	
### Solving for η
The sub-problem (eq. 8) can be solved independently for each $\eta_x$.  

Now the problem is to minimize, for each image x:
$$\begin{equation}
\mathbb{E}_{\mathcal{T}}\bigg[ ||\mathcal{F}_{\theta'}(\mathcal{T}(x))-\eta_x||^2_2 \bigg]
\end{equation}$$ 

Due to the mean squared error, it is easy to solve it by:
$$\begin{equation}
\eta^t_x \leftarrow \mathbb{E}_\mathcal{T}[\mathcal{F}_{\theta^t}(\mathcal{T}(x))]
\end{equation} \tag{9}$$

**This indicates that $\mathbf{\eta_x}$ is assigned with the average representation of $\mathbf{x}$ over the distribution of augmentation.**

### One-step alternation
But how does this relate to SimSiam? SimSiam can be approximated by one-step alternation between Eq. 7 and Eq 8.

 First, we approximate Eq. 9 by sampling the augmentation only once, denoted as $T'$, and ignoring $E_{\mathcal{T}}[\cdot]$:
$$\begin{equation}
\eta^t_x \leftarrow F_{\theta^t}(\mathcal{T}'(x))
\end{equation} \tag{10}$$
inserting this into Eq. 7, we have:
$$\begin{equation}
\theta^{t+1} \leftarrow \arg \min_\theta \mathbb{E}_{x,\mathcal{T}} \bigg[ ||\mathcal{F}_\theta(\mathcal{T}(x)) - \mathcal{F}_{\theta^t}(\mathcal{T}' (x))||^2_2 \bigg]
\end{equation} \tag{11}$$
Now $\theta^t$ is a constant in this subproblem. 

## Formulation with predictor
Our above analysis does not involve the predictor $h$.  
By definition, the predictor $h$ is expected to minimize: 
$$\begin{equation}
\mathbb{E}_z \big[ ||h(z_1) - z_2||_2^2\big]
\end{equation}$$

For any image $x$, the optimal solution to $h$ should satisfy:
$$\begin{equation}
h(z_1)=\mathbb{E}_z[z_2]=\mathbb{E}_\mathcal{T}[f(\mathcal{T}(x))]
\end{equation}$$


This term is similar to the one in Eq. 9.

In our approximation in Eq. 10. the expectation $\mathbb{E}_\mathcal{T}[\cdot]$ is ignored. 


**The hypothesis is that usage of $h$ may fill this gap and learn to approximate this average $\mathbb{E}_\mathcal{T}$.**  In other words, one head will output an image transformation and the other, with the head, will output an average of all those transformations. 



# Experiments
## Baseline settings
- **Backbone**. ResNet-50.
- **Projection MLP**. The projection MLP (in f) has BN applied to each fully-connected (fc) layer, including its output fc. Its output fc has no ReLU. The hidden fc is 2048-d. This MLP has 3 layers.
- **Prediction MLP.** The prediction MLP (h) has BN applied to its hidden fc layers. Its output fc does not have BN or ReLU. This MLP has 2 layers. The dimension of $h$’s input and output ($z$ and $p$) is $d = 2048$, and $h$’s hidden layer’s dimension is $512$, making $h$ a bottleneck structure.
- **Experimental setup**. We do unsupervised pre-training on the 1000-class ImageNet training set without using labels. The quality of the pre-trained representations is evaluated by training a supervised linear classifier on frozen representations in the training set, and then testing it in the validation set, which is a common protocol.


## Stop-gradient
![[Pasted image 20220203185659.png]]

## Predictor
Having no predictor is the same as not using stopgrad (see paper), so they collapse.
![[Pasted image 20220203185916.png]]

## Batch Size
![[Pasted image 20220203185922.png]]

## Batch Normalization
![[Pasted image 20220203190017.png|350]]
Both the failures in this table don't have anything to do with model collapse.

## Similarity Function
![[Pasted image 20220203190044.png|300]]

## Symmetrization
![[Pasted image 20220203190154.png|350]]

## Linear Classification ImageNet
![[Pasted image 20220206192131.png|600]]


___
#SelfSupervisedLearning 




