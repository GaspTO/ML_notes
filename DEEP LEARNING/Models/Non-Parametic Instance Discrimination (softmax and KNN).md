Paper: [***Unsupervised Feature Learning via Non-Parametric Instance Discrimination***](https://arxiv.org/pdf/1805.01978.pdf)


# Overview
This paper is introduces a **non-parametic softmax** (doesn't use weights)  and introduces the notion of classifying based on K-nearest neighbors. 

 This paper was created self-supervised learning
 

**TODO:** There is an approximation to the softmax made that I won't write now

# Methods
This is a unsupervised method.  The goal is to find *n* vectors, which are feature maps, that are then going to be used as similarity metrics for a new feature map. 
During the unsupervised training part, the vectors in the memory bank are scattered as much as possible, as seen in the following image, which summarizes the whole method.
![[Pasted image 20220804014650.png]]

## Non-parametic Softmax
By softmax, we are also including the classification layer (the mlp).
If you think about the *normal* parametic softmax, 
$$
P(i \mid \mathbf{v})=\frac{\exp \left(\mathbf{w}_{i}^{T} \mathbf{v}\right)}{\sum_{j=1}^{n} \exp \left(\mathbf{w}_{j}^{T} \mathbf{v}\right)}
$$
the $\mathbf{w}_j$ works as a vector for class $j$. And the $\mathbf{w}_j^T \mathbf{v}$ measure the similiary of the feature map to this vector. The higher the similarity, the higher the probability.

The non-parametic softmax works with a similar reasoning, but, instead of learning the vectors of comparison, $\mathbf{w}_j$, it uses some feature map of some image of reference, $\mathbf{v}_j$.

The new softmax becomes
$$
P(i \mid \mathbf{v})=\frac{\exp \left(\mathbf{v}_{i}^{T} \mathbf{v} / \tau\right)}{\sum_{j=1}^{n} \exp \left(\mathbf{v}_{j}^{T} \mathbf{v} / \tau\right)}
$$
An advantage of this method is that you can always add new vectors to this new softmax.

## Training
In this paper, the memory bank has as many *classes*/vectors as the whole training set. So each image belongs to its own class. This was something that was very confusing from the paper, but it's made more clear in their code.

The objective to maximize therefore is then
$$
\prod_{i=1}^{n} P_{\boldsymbol{\theta}}\left(i \mid f_{\boldsymbol{\theta}}\left(x_{i}\right)\right)
$$
Obviously, the reference vector for the class of $x_i$ is its own feature map, $v_i = f_\theta(x_i)$, so the dot product will be 1. But, the probability is influenced also by the similarity to other vector, which decreases if there are other vectors which are also similar to the feature map of $x_i$.  

In other words, the only way to maximize the probability of $P_{\boldsymbol{\theta}}\left(i \mid f_{\boldsymbol{\theta}}\left(x_{i}\right)\right)$ is to decrease the similarity of $f_\theta(x_i)$ to all other feature maps, which eventually leads to all of them scattering (being repelled by each other) as much as possible.

## Memory Bank
Obviously, every time we update our model, we don't recalculate the entire memory bank. The idea is to replace as we get new fresh feature maps calculated during the several training iterations.


## Classifying using K-nearest neighbors
After having features and a similarity measure, it's possible to classify in multiple ways. This paper introduces the KNN method, but also uses a linear SVM in its experiments.

With the KNN, to classify a new image, $\hat{x}$, its feature map, which is then compared against all the vectors in the memory bank using the cosine similarity. 
The top K features (as measured by the similarity) are used to decide the class of the new image.

For each class in the dataset, you take all the vectors in the memory bank and you test their similarity with the image's feature map. The exponentiation of each of the
top k similarities sum for each class. In the end, the class with the highest weight is attributed to the image. Mathematically,

$$
w_{c}=\sum_{i \in \mathcal{N}_{k}} \alpha_{i} \cdot 1\left(c_{i}=c\right)
$$
where $\alpha_{i}=\exp \left(s_{i} / \tau\right)$ and $\mathcal{N}_k$ is the top k nearest neighbors.


When classifiying, the memory bank labels are available, which makes the whole pointof the method a tidy useless, but, yet again, it's only during testing time, which pretty much tells us how good the features are. But to use them in a real supervised way we'd have to figure out something different.



___
#SelfSupervisedLearning #UnsupervisedLearning #ActivationFunction #KNN