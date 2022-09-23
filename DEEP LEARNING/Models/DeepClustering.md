Paper: [***Deep Clustering for Unsupervised Learning of Visual Features***](https://arxiv.org/pdf/1807.05520.pdf)

# References
- [Medium Article about Deep Clustering](https://sh-tsang.medium.com/review-deepcluster-deep-clustering-for-unsupervised-learning-of-visual-features-446d0abad74c)

# Overview
**DeepCluster** is a self-supervision approach for learning image representations. DeepCluster iteratively groups the features with a standard clustering algorithm, k-means, and uses the subsequent assignments as supervision to update the weights of the network.

![[deepclustering.png]]

# Methods
A network to classify images has two parts:
1. a mapping function that outputs a feature map, $f_\theta(\mathbf{x})$.
2. a classifier, usually linear, $g_w$, that receives a feature map and outputs a classification score $g_w(f_\theta(\mathbf{x}))=\mathbf{\hat{y}}$

This optimization problem can be described by:
$$\begin{equation}
\min_{\theta,W} \frac{1}{N} \sum_{n=1}^{N} l(g_w(f_\theta(x_n)),y_n)
\end{equation} \tag{1}$$


A random initialized AlexNet achieves 12% in imagenet when the chance is 0.1%.  The good performance of random convnets is intimately tied to their convolutional structure which gives a strong prior on the input signal. 


This features can be seen as *clustering* together similar images, through the natural priors of the filters, by giving them (more) similar feature maps.

DeepClustering exploits this weak signal and augments it.  In other words, it incentives more clustering of the features already close to each other. It does this by using a clustering algorithm. They use k-means, but state that preliminary results with others seemed to work well. The classifier they train with self-supervised learning is not the same used later.




## K-means
In k-means a set of vectors is clustered into k distinct groups based on a geometric criterion like the distance. More precisely, k-means jointly learns a $d \times k$ centroid matrix $C$ and the cluster assignments $t_n$ of each image $n$ by solving the following problem:

$$\begin{equation}
\min_{C\in \mathcal{R}^{d \times k} } \frac{1}{N}\sum_{n=1}^{N} \min_{y_n \in {0,1}^k} ||f_\theta(x_n)-Cy_n||^2_2
\end{equation} \tag{2}  $$
Where $y_n^\intercal 1_k = 1$ and $C$ is the centroid matrix.

In other words, it assigns a centroid to each point. Each of these has associated with it a label. 

Solving this problem gives the optimal centroid matrix $C^*$ and label assignments to each centroid $(y^*_n)_{n\leq N}$. These assignments are then used as pseudo-labels.

The number of centroids does not need to be the same as the labels of the dataset we are trying to improve on.

## Iteration
Overall, DeepCluster alternates between clustering the features to produce pseudo-labels using Eq. 2 and updating the parameters of the convnet by predicting these pseudo-labels using Eq. 1 using the labels created by the clustering.

This type of alternating procedure is prone to trivial solutions; we describe how to avoid such degenerate solutions in the next section.


## Avoid Trivial Solutions
1. **Empty Clusters.** An optimal decision boundary is to assign all of the inputs to a single cluster. This issue is caused by the absence of mechanisms to prevent from empty clusters and arises in linear models as much as in convnets. To fix this, we randomly select a non-empty cluster and use its centroid with a small random perturbation as the new centroid for the empty cluster. We then reassign the points belonging to the non-empty cluster to the two resulting clusters.

2. **Trivial parametrization**. If the vast majority of images is assigned to a few clusters, the parameters $\theta$ will exclusively discriminate between them. In the most dramatic scenario where all but one cluster are singleton, minimizing Eq. 1 leads to a trivial parametrization where the convnet will predict the same output regardless of the input. This issue also arises in supervised classification when the number of images per class is highly unbalanced. A strategy to circumvent this issue is to sample images based on a uniform distribution over the classes, or pseudo-labels. This is equivalent to weight the contribution of an input to the loss function in Eq. 1 by the inverse of the size of its assigned cluster.

## Note on clustering
The labels learned are not the correct ones, but the centroids. Learning the labels means mapping the same features to the same centroid. This means that, after using self-supervised, you have to remove the classifier and train a new one that will learn to map to the correct *buckets*.

#  Experiments
There's several experiments in the paper we put here the ones where, after the data has been trained with self-supervised, a new linear classifier is trained with supervised data.

## Classifier per Layer
They train a supervised classifier on imageNet on top of each layer in AlexNet and measure the performance
![[Pasted image 20220202011741.png]]



___
#SelfSupervisedLearning  #Vision 