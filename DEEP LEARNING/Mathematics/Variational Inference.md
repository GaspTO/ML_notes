# References
1. [Arxiv paper explaining what it is and examples](An Introduction to Variational Inference)

2. [Wikipedia page](https://en.wikipedia.org/wiki/Variational_Bayesian_methods).

3. Pattern Recognition chapter 10.


# Overview
The purpose of VI is to provide an analytical approximation of the posterior probability density $p(\mathbf{Z}|\mathbf{X})$ for statistical inference over the latent variables. VI enables efficient computation of a lower bound to the marginal probability density, or the evidence. The idea is that a higher marginal likelihood is indicative of a better fit to the observed data by the chosen statistical model. Additionally, VI addresses the approximation problem by choosing a probability density function q for the latent variable Z from a tractable family

They are typically used in complex statistical models consisting of observed variables (usually termed "data") as well as unknown parameters and latent variables.

the parameters and latent variables are grouped together as "unobserved variables". Variational Bayesian methods are primarily used for two purposes:

1.  To provide an analytical approximation to the [posterior probability](https://en.wikipedia.org/wiki/Posterior_probability "Posterior probability") of the unobserved variables, in order to do [statistical inference](https://en.wikipedia.org/wiki/Statistical_inference "Statistical inference") over these variables.
2. To derive a [lower bound](https://en.wikipedia.org/wiki/Lower_bound "Lower bound") for the [marginal likelihood](https://en.wikipedia.org/wiki/Marginal_likelihood "Marginal likelihood") (sometimes called the "evidence") of the observed data (i.e. the [marginal probability](https://en.wikipedia.org/wiki/Marginal_probability "Marginal probability") of the data given the model, with marginalization performed over unobserved variables).


## Problem
In variational inference, the posterior distribution over a set of unobserved variables $\mathbf{Z} = \{Z_{1}\dots Z_{n}\}$ given some data $\mathbf{X}$  is approximated by a so-called variational distribution, $Q(\mathbf{Z})$:
$$\begin{equation}
P(\mathbf{Z}|\mathbf{X}) \approx Q(\mathbf{Z})
\end{equation}$$

$Q(\mathbf{Z})$ is restricted to be part of a family of simpler distributions than $P(\mathbf{Z}|\mathbf{X})$ (like a gaussian distributions), selected with the intention of making $Q(\mathbf{Z})$ similar to the true posterior,  $P(\mathbf{Z}|\mathbf{X})$.

To *approximate*  a distribution, you need a similarity (or dissimilarity) function, $d(Q;P)$  and, therefore, inference means selecting the distribution $Q(\mathbf{Z})$ that minimizes $d(Q;P)$ . 


## Similarity Function - KL divergence
The most common dissimilarity function used is the **KL divergence** of P from Q:
$$\begin{equation}
D_{KL}(Q||P) = \sum_{\mathbf{Z}}Q(\mathbf{Z})\log \frac{Q(\mathbf{Z})}{P(\mathbf{Z}|\mathbf{X})}
\end{equation}$$
Note that  Q and P are reversed from what one might expect.


## Intractability
As said, the goal is to approximate $P(\mathbf{Z}|\mathbf{X})$. In simpler cases, this can be done by:
$$\begin{equation}
{P(\mathbf{Z}|\mathbf{X})} = 
\frac{P(\mathbf{Z}|\mathbf{X}) P(\mathbf{Z})}
	{P(\mathbf{X})} =
\frac{P(\mathbf{Z}|\mathbf{X}) P(\mathbf{Z})}
	{\int_{Z} P(\mathbf{X},\mathbf{Z}) d\mathbf{Z}}
\end{equation}$$

But in more complicated cases, the denominator is untractable since the combinatorial space of $\mathbf{Z}$ (the combination of each variable in the vector $\mathbf{Z}$) can be very large. 

That's why we are seeking an approximation $Q(\mathbf{Z}) \approx P(\mathbf{Z}|\mathbf{X})$.


## Evidence lower bound
The KL difference can be shown to be 
$$\begin{equation}
D_{KL}(Q||P) = \mathbb{E}_Q [\log Q(\mathbf{Z})-\log P(\mathbf{Z},\mathbf{X})] + \log P(\mathbf{X})
\end{equation}$$
see the full derivation [here](https://en.wikipedia.org/wiki/Variational_Bayesian_methods).

This, can then be arranged to be:
$$\begin{equation}
\log P(\mathbf{X}) =
D_{KL}(Q||P) - \mathbb{E}_Q [\log Q(\mathbf{Z})-\log P(\mathbf{Z},\mathbf{X})] = 
D_{KL}(Q||P) + \mathcal{L}(Q)
\end{equation}$$

$\mathcal{L}(Q)$ is know as (negative) variational energy and also as Evidence lower bound (ELBO).

$\log P(\mathbf{X})$ does not change, it is constant with respect to Q, so, if the second term $\mathcal{L}(Q)$ increases, the first $D_{KL}(Q||P)$ has to decrease. In other words, if we maximize the variational energy, the dissimilarity decreases - the distribution $Q$ becomes closer to $P$.

Also, because the dissimilarity is always a positive term, the variational energy is a lower bound of $\log P(\mathbf{X})$  (therefore the name ELBO).

You can also reaarange terms and get
$$\begin{equation}
\mathcal{L}(Q) = \mathbb{E}[\log P(\mathbf{X}|\mathbf{Z})] - D_{KL}(Q(\mathbf{Z})||P(\mathbf{Z}))
\end{equation}$$
Thus, the ELBO is the sum of the expected log likelihood of the data and the KL-divergence between the prior and approximated posterior probability density. The expected log likelihood describes how well the chosen statistical model fits the data. The KL-divergence encourages the variational probability density to be close to the actual prior. Thus, the ELBO can be seen as a regularised fit to the data.

## Mean field variational family
In the mean field variational family for VI, the latent variables are assumed to be mutually independent—each governed by a distinct factor in the variational probability density. This assumption greatly simplifies the complexity of the optimization process. A generic member of the mean field variational family is
$$\begin{equation}
q(\mathbf{Z}|\mathbf{X}) = \prod_{j=1}^m(q_j(\mathbf{Z_j}))
\end{equation}$$
where m is the number of latent variables. The observed data $\mathbf{X}$ does not appear in this equation, therefore any probability density from this variational family is not a model of the data. Instead, it is the ELBO, and the corresponding KL-divergence minimization problem, which connects the fitted variational probability density to the data and model

# Nomenclature/Concepts
- $P(\mathbf{x})$: marginal likelihood or evidence. For many models, this evidence integral depends on the selected model and is either unavailable in closed form or requires exponential time to compute
- $P(\mathbf{z})$: prior
- **Evidence Lower Bound**: It is the lower bound on the evidence ($P(\mathbf{x})$).

# Neat Trick Observation
There's this idea I've been seeing while learning about this. 
Say you have a distribution $P(\mathbf{A},\mathbf{B})$ that you can't compute. You can always break it in a way that it ends up as an average: $P(\mathbf{A}|\mathbf{B})P(\mathbf{B}) = \mathbb{E}_{B \sim P(\mathbf{B})} [P(\mathbf{A}|\mathbf{B})]$  and then you can simply do sampling.
	