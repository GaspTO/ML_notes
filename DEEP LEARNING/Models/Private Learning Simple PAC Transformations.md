**Paper**: [***The Art of Labeling: Task Augmentation for Private (Collaborative) Learning on Transformed Data***](https://eprint.iacr.org/2021/601.pdf)

# Overview
This paper talks about private learning, and how can we transform the original data samples before sending them them to a host server that will train our neural network. The focus is in trying to transform the data, while allowing it to be easily trainable. 

The paper bases itself in a theoretic concept called PAC security to prove those transformations have some degree of security.

# PAC security
To be done...


# Transformations
They introduce 3 private transformations. A private transformation is one that happens to the data before being sent to the server. One for fully connected neural networks, another for convolutional neural networks and, lastly, another one for transformers.

They also briefly explain how transfer learning can be done using private transformations.

## Fully Connected Networks
* $T(x) = \sigma (xW)$
* $W$ is a random matrix, where each entree is randomly selected from {-1, 0, 1}. 
* It is noted that the expectation of each entry is 0 and based on the generalization of the *Johnson-Lindenstrauss Lemma*. a random matrix of zero mean has the potential to preserve the transformed sample pairwise distance, which, by preservating the geometric geometry. should facilitate training.


## Transformers
* Reminder: In the transformer, the different segments of the input $\mathbf{x} = [ x_1 , ... , x_p]$, with $x_i \in \mathbb{R}^{d_p}$ are multiplied by an embedding matrix $E$ with dimensions $d_p \times d_p$.  As shown:

![[Pasted image 20220322131602.png|350]]

* The transformation is applying a different linear transformation to each of the segments:
$$\begin{equation}
\mathbf{x}_t \rightarrow 
	\begin{bmatrix} 
				x_1 \tilde{W}_1 \\ x_2\tilde{W}_2 \\ ... \\x_p\tilde{W}_p \\ 
	\end{bmatrix}
\end{equation}$$
with a randomly selected $\tilde{W}_i \in \{-1, 0, +1\}^{d_p \times d_p}$.

* To preserve the representation capacity of a Transformer, we add an
$E_i$ learnable fully connected layer for each $x_i\tilde{W}_i$ and feed the result
to the Transformer.

* Given that we have added this learnable matching layer, we remove the 𝐸 embedding
matrix and, essentially, replace it by $p$  $E_𝑖$ matrices.

![[Pasted image 20220322142645.png|350]]


## Convolutional Neural Networks
![[Pasted image 20220322152625.png|300]]

* Transforms every $k \times k$ squares in the input image into $1 \times k^2$ patches. It's similar to the patches of the transformer, the only difference is that these ones can be overlapped.
* Convolve those using a $k^2 \times k^2$ random $\tilde{W}_i$ matrix, obtaining $z_i$, which maintains locality within the window. Each matrix is different.
* Depending on the stride 𝑠, different numbers of $1 \times k^2$ vectors are generated, one for each position of the pretend kernel.
* . In addition, since we have already generated the convolution windows over the input in the transform, we modify the first convolutional layer of the CNN to only perform the multiplication of the provided input by a number of learnable weight kernels as shown in the next figure. we assume the original CNN has $𝑢$ learnable kernels in its first layer.

![[Pasted image 20220322155145.png|350]]
 * The second and subsequent layers in the CNN are unchanged.















___
#PrivateLearning






