Paper: [***Learning a Similarity Metric Discriminatively, with Application to Face Verification***](http://yann.lecun.com/exdb/publis/pdf/chopra-05.pdf)

# Overview
The idea of this paper is to learn to map faces to a target space (output a representation vector). Pictures of the faces of the same person should have similar representations, while different faces should be different.

It is particularly good when we do not know the number of classes at training time or when there are a lot of them (like in the order of thousands or millions...).

These models are part of *energy-based models* (EBMs). EBMs are used in situations where the energies for various configurations must be compared in order to make a decision (classification, verification, etc). The advantage of EBMs over traditional probabilistic models, particularly generative models, is that there is no need for estimating normalized probability distributions over the input space. The absence of normalization saves us from computing partition functions that may be intractable. It also gives us considerably more freedom in the choice of architectures for the model.

It uses a [[Siamese Architecture]], each networks receives a face picture. $X_1$ and $X_2$, and output a measure of similarity, $E_W(X_1,X_2)$. The output of each network is given by $G_W(X)$. The objective is therefore to find $E_W(X_1,X_2) = ||G_W(X_1) - G_W(X_2)||, where it should small if the pictures are similar and big otherwise.

![[Siamese Architecture.png]]

# Training (Contrastive) Loss
Given the parameters $W$ and a tuple $(Y,X_1,X_2)$, composed by a label about whether the pictures $X_1$ and $X_2$ correspond to the same person, the loss is
$$\begin{equation}
\mathcal{L}(W) = \sum_{i=1}^{P} L(Y,X_1,X_2;W) =\sum_{i=1}^{P} (1-Y)L_G(E_W(X_1,X_2)^i) +  YL_I(E_W(X_1,X_2)^i)
\end{equation}$$

$L_G$ 4 is the partial loss function for a genuine par, $L_1$ the partial loss function for an impostor pair, and $P$ the number of training samples. $L_G$ and $L_I$ should be designed in such a way that the minimization of $L$ will decrease the energy of genuine pairs and increase the energy of impostor.

There are some conditions that should be met for this to work ($X_2'$ is an impostor picture).

- **Condition 1.** $\exists m > 0$, such that $E_W(X_1,X_2) + m < E_W(X_1,X_2')$ .  $m$ is called margin and should be added to the loss function to prevent trivial solutions from being found.

-  **Condition 2.**  We define $E^G_W$ as the loss for a genuine pair and $E^I_W$ for an impostor one, and also $H(E^G_W,E^I_W) = L_G(E^G_W) + L_I(E^I_W)$ as the loss function for two pairs. We will assume that H is convex in its two arguments. We also assume that there exists a $W$ for single training sample such that condition 1 is satisfied.  The minima of $H(E^G_W,E^I_W)$ should be inside the half plane $E^G_W + m < E^I_W$. This condition clearly guarantees that when we minimize $H$ with respect to $W$, the machine is driven to a region where the solution satisfies condition 1. 

-  **Condition 3.** The negative of the gradient $H(E^G_W,E^I_W)$ on the margin line $E^G_W + m = E^I_W$ has a positive dot product with the direction $[-1,1]$.


The paper also recommends using the L1 loss and the L2 loss. *Indeed, if the energy were the square norm of the difference between the output vectors of the two patterns, the gradient of the energy with respect to the parameter would vanish as the energy approached zero. This would create a dangerous plateau in the loss function. This could lead to failure of the machine to learn in cases where the two images are impostors and the corresponding energy is near zero.*

They use convolutional neural networks as the base network.

#General #Vision #EnergyBasedModel #ContrastiveLearning 