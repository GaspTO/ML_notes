nPaper: [***Stochastic Segmentation Networks: Modelling Spatially Correlated Aleatoric Uncertainty***](https://arxiv.org/pdf/2006.06015.pdf)

# Overview
It's a stochastic network for segmentation - it does not produce the same segmentation for the same input every time it is ran.


# Method 
* For $S$ pixels and $C$ classes to classify per pixel,  the idea is to sample logits to obtain a similar quantity as the log softmax:
$$\begin{equation}
-\log \int p(\mathbf{y}|\mathbf{\eta}) p(\mathbf{\eta}|x) d\eta \approx - \log \frac{1}{M} \sum_{m=1}^{M} p(\mathbf{y}|\eta^{(m)}), 
\quad \eta^{(m)}|x \sim \mathcal{N} (\mu(x),\Sigma(x))
\end{equation}$$
Where $M$ is the number of samples used to for this estimation; $\eta$  is the all the logits vector of every *pixel*. We will use $\eta_i$ to denote the logits vector of *pixel* $i$.  Also, $\mu \in \mathbb{R}^{S \times C}$ and $\Sigma(x) \in \mathbb{R}^{(S \times C)^2}$.


* To obtain a loss function, they use the $\text{logsumexp}$ operator:
	$$\begin{equation}
	- \text{logsumexp}_{m=1}^{M} \bigg(\sum_{i=1}^S \log(p(y_i|\eta_i^{(m)}))\bigg) + \log(M), \quad  \eta^{(m)}|x \sim \mathcal{N}(\mu(x),\Sigma(x))
	\end{equation}$$
* $\mu(x)$ is calculated by a neural network
* $\Sigma(x) = \mathbf{P} \mathbf{P}^\intercal + \mathbf{D}$,   where $\mathbf{P} \in \mathbb{R}^{(S \times C) \times R}$ and $D \in \mathbb{R}^{(S \times C)}$ (both 2-dim matrices) and calculated by a neural network. $R$ is an hyperparameter called rank and is evaluated in more detail in the paper.


# Discussion
**In addition to being able to sample repeatedly after inference, another advantage of outputting a full distribution is the ability to manipulate samples post-inference**. Since the covariance matrix has entries which are separable per class, by scaling only the part of the matrix relating to a given class, we are able to manipulate samples to increase or reduce the presence of that class. This can be used to correct possible mistakes or adjust borders, as shown in Figure 6. Similarly, we can trade sample diversity for quality by scaling the temperature of the entire distribution.
![[Pasted image 20220222184136.png]]
