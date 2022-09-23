Paper: [***A Disentangling Invertible Interpretation Network for Explaining Latent Representations***](https://openaccess.thecvf.com/content_CVPR_2020/papers/Esser_A_Disentangling_Invertible_Interpretation_Network_for_Explaining_Latent_Representations_CVPR_2020_paper.pdf)

[Github](https://github.com/CompVis/iin)

# Overview
This paper introduces a methodology that can be used to map the feature maps of some layer, in any network, to another representation which is disentangled. The disentangled representation is a factorized one, where each factor represents a semantic concept.
The way this is done is by training an autoenconder that receives as an input the feature map, and encondes it to the factorized representation.  

![[Pasted image 20220830004202.png|350]]



# Method
## Notation and Definitions
* $f$ is the network to be interpreted.
* *$E(x) = z$ is a part of the network $f$ that returns feature maps with dimensions $N = H \times W \times C$. 
* $f(x) = G(E(x))$
* $\tilde{z} = (\tilde{z}_k)^{K}_{k=0} \in \mathbb{R}^N$     -> represents a semantic concept
* $\tilde{z}_0$ is the residual factor, that captures the rest of the semantic concepts
* $\tilde{z}_k \in \mathbb{R}^{N_k}$ , with $\sum_{k=0}^{K} N_k = N$
* $T(z) = \tilde{z}$      -> the forward map that will transform the feature map into a interpretable representation
* $T^{-1}(\tilde{z}) = z$   
* $z \rightarrow z^* := T^{-1}(T(z)* )$        -> $z^*$ is the result of converting $z$ to $\tilde{z}$, changing some semantic factors ($\tilde{z}_k$) and returning it back to $z$. See the next picture where, for example for the first, the concept $\tilde{z}_1$ which is the concept of digit is replaced from the images on the top row to the images of the leftmost column. As you follow the rows, the digit changes, but the color is maintained.
![[Pasted image 20220830001642.png]]

* $(x^a,x^b) \in F$      -> data pairs that have in common the same instance of the concept $F$. For example, if $F$ refers to animal,  $(x^a,x^b)$ refer the same animal.
* $\tilde{z}^a = T(E(x^a))$


## Disentangling Interpretable Concepts
**How do we create the interpretable concepts?**

* Each factor, $\tilde{z}_k$,  of $\tilde{z}$  should be associated with an interpretable concept. It should be easy to replace one $\tilde{z}_k$ of an image by one of another image. This independence of factors, suggests
$$\begin{equation}
p(\tilde{z})=\prod_{k=0}^K p(\tilde{z}_k)
\end{equation}$$
* We assume the distribution of $\tilde{z}$ to be a gaussian centered in 0:
$$
p(\tilde{z}) = \prod_{k=0}^K \mathcal{N}(\tilde{z}_k|0,\mathbf{1})
$$
* To disentangle a concept $F$ out of $\{1, ... , K\}$ concepts, we train data pairs $(x^a,x^b)$ which contain the same instance of the concept $F$. For example, if $F$ is the semantic concept referring to animal, then the pairs $(x^a,x^b)$ should regard the same animal. We represent this sampling of pairs by $(x^a , x^b ) \sim p(x^a , x^b |F)$

* For a given pair $(x^a , x^b ) \sim p(x^a , x^b |F)$, the corresponding factorized representations, $\tilde{z}^a$ and $\tilde{z}^b$, must now (i) mirror the semantic similarity of $(x^a,x^b)$ in its $F^{th}$ factor between pairs, and (ii) be invariant in the remaining factors. This is expressed by a positive correlation factor $\sigma_{a,b} \in (0,1)$ for the F-th factor between pairs,
$$
\tilde{z}^b_F \sim \mathcal{N}(\tilde{z}^b_F|\sigma_{a,b}\tilde{z}^a_F,(1-\sigma_{a,b}\mathbf{1})))
$$
and no correlation for the remaining factors between pairs,
$$
\tilde{z}^b_k \sim \mathcal{N}(\tilde{z}^b_k|0,1) 
$$ (small note: in their code, it seems that $\sigma_{a,b}$ is a constant. Maybe we are supposed to choose it as an hyper-parameter. Which makes sense, we are giving the freedom for the factors to vary. The stricter we are, the higher the loss when they are different, as we'll see).

* They compute the likelihood of pairs $(z^a,z^b) = (E(x^a),E(x^b))$. We compute the likelihood with the absolute value of the Jacobian determinant of $T$, denoted $|T'(·)|$, as
  $$ \begin{align}
  p(z^a,z^b|F) &= p(z^a)p(z^b|z^a,F)\\
	  &=|T'(z^a)|p(T(z^a)) \cdot |T'(z^b)|p(T(z^b))p(T(z^b)|T(z^a))
  \end{align}$$
  * They apply the negative log-likelihood to this likelihood utilizing the previous  3 equations and come up with the error function:
$$ \begin{align}
    \mathbb{l}(z^a,z^b|F)  &= \sum_{k=0}^K ||T(z^a)_k||^2 - \log|T'(z^a)| \\ &+ \sum_{k\neq F}||T(z^b)_k||^2 - \log|T'(z^b)| \\ &+  \frac{||T(z^b)_F - \sigma_{a,b}T(z^a)_F||^2}{1-\sigma_{a,b}}
\end{align}$$
  * which is optimized over training pairs $(x^a,x^b)$ for all semantic concepts $F \in \{1, ... , K\}$:
$$
	\mathcal{L} = \sum_{F=1}^K \mathbb{E}_{(x^a,x^b)\sim p(x^a,x^b|F)} \mathbb{l}(E(x^a),E(x^b)|F)
$$

## Obtaining Semantinc Concepts
### Estimating Dimensionality of Factor
* So far, we know how to train the map $T$, but we have to decide the size of each component first. To do this, we take semantic pairs $(x^a,x^b)$ and compute, for each concept, the relative score 
$$
s_F = \sum_i \frac{\text{Cov}(E(x^a)_i,E(x^b)_i)}{\sqrt{\text{Var}(E(x^a)_i),\text{Var}(E(x^b)_i)}}
$$
$s_F \in [-N, N]$; $i$ is each neuron in the feature map we are interpreting. 

* We set the residual factor, $\tilde{z}_0$, to have a dimension of $N$. Using the maximum score N for the residual factor $\tilde{z}_0$ ensures that all factors have equal dimensionality if all semantic concepts are captured by $E$.

* To calculate the dimensionality $N_F$ of $\tilde{z}_F$ we do
$$
N_F = \frac{\exp s_F}{\sum_{k=0}^K \exp s_k} N  
$$
For example,
![[Pasted image 20220830032308.png|400]]

## Implementation
**There is a huge difference between what's in the paper, and what is implemented.** If we follow the dimensionality estimation following the paper, it won't work. They don't use $s_F$ to calculate $N_F$, but $\frac{s_f}{N}$, so that the exponential is done over values between -1 and 1.

There are also some differences regarding how to calculate the *mean* and *var* and how to calculate $s_F$, where, in their code, they sum the covariances and only then average by the *variance* of the entire data of the factor $F$.