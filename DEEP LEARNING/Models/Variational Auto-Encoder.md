Paper: [***Auto-Encoding Variational Bayes***](https://arxiv.org/pdf/1312.6114.pdf)

# References
1. [Arxiv paper explaining what it is and examples](https://arxiv.org/pdf/2108.13083.pdf)
2. [Stanford slides](http://cs231n.stanford.edu/slides/2017/cs231n_2017_lecture13.pdf)


# Overview
This is a [[Variational Inference|Variational Bayesian Method]] . 

It is similar to [[Autoencoders]], but both the encoder and decoder output a distribution, instead of a specific value.

The overrall idea is to teach the decoder how to *decode* the latent variables (the codification) in a way that maximizes the likelihood of the data observed. 

![[Pasted image 20220131004449.png|400]]

Because the variational autoencoder outputs distributions, it can be used to generate new data.


# Problem
Let us consider some dataset $\mathbf{X} =\{\mathbf{x}^{(i)}\}^N_{i=1}$ is generated from underlying unobserved (latent) representation $\mathbf{z}$.

![[Pasted image 20220130030515.png|300]]

Intuition (remember from autoencoders!): x is an image, z is latent factors used to generate x: attributes, orientation, etc.

We want to estimate the true parameters $\theta^*$of this generative model.

Choose prior $p_\theta(\mathbf{z})$ to be simple, e.g. Gaussian, $\mathcal{N}(\mathbf{z},0,\mathbf{I})$.

Conditional p(x|z) is complex (generates image) => represent with neural network


Problem: If we were to train this model by maximizing the maximum  likelihood, we would need to compute an intractable integral (sum) since the complexity of the latent space is too big:
$$\begin{equation}
p_\theta(\mathbf{x}) = \int p_\theta(\mathbf{z}) p_\theta(\mathbf{x}|\mathbf{z})
\end{equation}$$

Since $p_\theta(\mathbf{x})$ is intractable, so is the posterior density (so using the [[Expectation Maximization|EM algorithm]] cannot be done):
$$\begin{equation}
p_\theta(\mathbf{z}|\mathbf{x}) = 
\frac
{p_\theta(\mathbf{z})
p_\theta(\mathbf{x}|\mathbf{z})}
{p_\theta(\mathbf{x})}
\end{equation}$$

# Solution
If we approximate the exact posterior, $p_\theta(\mathbf{z}|\mathbf{x})$ with a function $q_\phi(\mathbf{z}|\mathbf{x})$ with parameters $\phi$ , we can write the original goal to maximize the marginal likelihood as (see [[Variational Inference]]):

$$\begin{equation}
\log p_\theta(\mathbf{x}^{(i)}) = D_{KL}(q_\theta(\mathbf{z}|\mathbf{x}^{(i)}) || p_\theta(\mathbf{z}|\mathbf{x}^{(i)})) + \mathcal{L(\theta, \phi; \mathbf{x}^{(i)})}
\end{equation}$$

where 


$$\begin{align}
\mathcal{L}(\theta,\phi;\mathbf{x}) 
&= \mathbb{E}_{q_\phi(\mathbf{z}|\mathbf{x^{(i)}})}[-\log q_\phi(\mathbf{z}|\mathbf{x^{(i)}}) + \log p_\theta(\mathbf{x^{(i)}},\mathbf{z})]  \\
&=
D_{KL}\big(q_\phi(\mathbf{z}|\mathbf{x}^{(i)})||p_\theta(\mathbf{z})\big) + \mathbb{E}_{q_\phi(\mathbf{z}|\mathbf{x}^{(i)})}[\log p_\theta(\mathbf{x^{(i)}}|\mathbf{z})]
\end{align}$$


we call $q_\phi(\mathbf{z}|\mathbf{x})$  the probabilistic encoder.

The second term evaluates the reconstruction error just like a normal autoencoder. The KL difference in the first term can be seen as a regularization term.

We draw independent samples, z i , from the variational distribution, Qφ(Z|X), and then compute the average of the function evaluated at these samples:
$$\begin{equation}
\mathbb{E}_{z \sim q_\phi(\mathbf{z}|\mathbf{x}^{(i)})} \simeq \frac{1}{L} \sum_{l=1}^{L}\log p_\theta(\mathbf{x}^{(i)}|\mathbf{z}^{(i,l)})
\end{equation}$$


# Gaussian Encoder/Decoder
Both the encoder and decoder are supposed to model a distribution, so they output the parameters of that distribution. 

We assume that the variational approximate posterior, $q$, models a multivariate gaussian (outputting two parameters: mean, $\mu$, and standard deviation, $\sigma$). 

Once we have the distributions, we can sample the latent variables $\mathbf{z}$ from the encoder and the output data $\mathbf{x}$ from the decoder.

The prior, $p(\mathbf{z})$, is given by the multivariate gaussian $\mathcal{N}(\mathbf{z},0,\mathbf{I})$.

The sampled loss is therefore:
$$\begin{align}
\mathcal{L}(\theta,\phi;\mathbf{x}) 
&\simeq
\frac{1}{2}\sum_{j=1}^{J}[1 + \log((\sigma_j^{(i)})^2)- (\mu_j^i)^2 - (\sigma_j^{(i)})^2]  \\
&+ 
\frac{1}{L}\sum_{l=1}^{L}\log p_\theta(\mathbf{x}^{(i)}|\mathbf{z}^{(i,l)})
\end{align}$$
where $J$ is the dimensionality of $\mathbf{z}$.

(See the original paper to know how to get to that formula, which pretty much the $D_{KL}$ of a multivariate gaussian).


![[Pasted image 20220130172522.png|300]]![[Pasted image 20220130172512.png|300]]



# Summary
(This is my own understanding of the problem).

The problem starts with only the decoder. There's a distribution $P(\mathbf{z})$ and the goal is to learn a fucntion that connects every $\mathbf{z}$ to an $\mathbf{x}$ in a way that the probability of a particular $\mathbf{x}$ is higher when $P(\mathbf{x})$ is also higher. 


![[Pasted image 20220130172522.png|300]]


The problem is that the only way to this is by evaluating every single mapping from $\mathbf{z}$ to $\mathbf{x}$ which is impossible since the latent space is infinitely big.

the solution found is to approximate the posterior which is also intractable.

it turns out that the KL difference can be approximated by Monte Carlo sampling (which is why it is so popular) and can be decomposed into two parts, one of them being the (marginal) likelihood of the data.

If we rearrange this we can get a loss part that acts as a lower bound on the likelihood of the data and that can then be decomposed in:

$$\begin{equation}
\mathcal{L}(\theta,\phi;\mathbf{x}) 
= \mathbb{E}_{q_\phi(\mathbf{z}|\mathbf{x}^{(i)})}[\log p_\theta(\mathbf{x^{(i)}}|\mathbf{z})]
+
D_{KL}\big(q_\phi(\mathbf{z}|\mathbf{x}^{(i)})||p_\theta(\mathbf{z})\big)
\end{equation}$$

1. The term that replaced the complete evaluation of the whole $\mathbf{z}$ space per sampling. It samples $\mathbf{z}$s and the evaluates the data likelihood.
2. A regularizing term that ground the learned function $q(\mathbf{z}|\mathbf{x})$ to the prior with respect to $\mathbf{z}$. In other words, the $\mathbf{z}$ distribution for each $\mathbf{x}$ approximates $p(\mathbf{z})$.

When updating the gradient, the derivative of $\phi$ directs the $q$ function towards the prior and towards maximizing the likelihood of the current data point in the batch;
the derivative of $\theta$ directs it to maximize the likelihood of the current data point in the batch.


# Questions
I wonder what would happen if we chose other similarity functions. Maybe we wouldn't get the regularizer term, which is something different here from autoencoders that just get the other term.