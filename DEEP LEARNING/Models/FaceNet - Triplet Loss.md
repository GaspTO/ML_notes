Paper: ***FaceNet: A Unified Embedding for Face Recognition and Clustering***

Similar to [[Contrastive Learning with Siamese Networks]].

# Triplet Loss
It uses triplets, one image called anchor, a second that matches the anchor $x^p$ and the third which doesn't $x^n$.

The objective is, for all triplets,
$$\begin{equation}
\lvert \lvert f(x_i^a) - f(x_i^p) \rvert \rvert^2_2 + \alpha < \lvert \lvert f(x_i^a) - f(x_i^n) \rvert \rvert^2_2
\end{equation} \quad \quad \forall (x_i^a,x_i^n, x_i^p) \in \mathcal{T}$$ 
Where $\mathcal{T}$ is the space of all triplets.

The  $\alpha$ is to avoid the $f$ function to map everything to trivial solutions like everything being the same.

The paper normalizes the embeddings to norm of 1, $\lvert \lvert f(x) \rvert \rvert^2_2$.

Therefore, the loss is for $N$ triplets

$$\begin{equation}
\sum_i^N \bigg[
\lvert \lvert f(x_i^a) - f(x_i^p) \rvert \rvert^2_2 + \alpha - \lvert \lvert f(x_i^a) - f(x_i^n) \rvert \rvert^2_2 \bigg]
\end{equation}$$


# Triplet Selection
We want triplets that violate the objective stated previously. Choosing triplets randomly might lower the frequency of chosen ones that do not contribute to the network. 
Ideally, for an anchor $x_i^a$, we want a hard positive $\arg \max_{x_i^p} \lvert \lvert f(x_i^a) - f(x_i^p) \rvert \rvert^2_2$ and a hard negative $\arg \min_{x_i^n} \lvert \lvert f(x_i^a) - f(x_i^n) \rvert \rvert^2_2$

It is infeasible to compute the argmin and argmax across the whole training set. Additionally, it might lead to poor training, as mislabelled and poorly imaged faces would dominate the hard positives and negatives. There are two obvious choices that avoid this issue:
1. Generate triplets offline every n steps, using the most recent network checkpoint and computing the argmin and argmax on a subset of the data.
2. Generate triplets online. This can be done by selecting the hard positive/negative exemplars from within a mini-batch.

# Evaluation
The evaluation is done by measuring the distance between photos and if they are below a certain threshold $d$, they are considered to belong to the same identity.

___
#General #Vision #EnergyBasedModel #ContrastiveLearning 
