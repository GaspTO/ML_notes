Paper: [***Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift***](https://arxiv.org/pdf/1502.03167.pdf)

# Overview
State of the Art in Imagenet. 
It take the [[Inception v1 - GoogLeNet]] and adds this new technique called batch normalization, which helps stabilizing the inputs in between layers. The resulting network is called Inception-v2.

### Problem
$$\begin{equation}
l = F_2(F_1(u,\Theta_1),\Theta_2)
\end{equation}$$
$F_1$ and $F_2$ are arbitrary functions. If $x = F_1(u,\Theta_1)$, we see that the input of $F_2$ is always changing, which makes it challenging to learn function $F_2$.

### Observation
It has been known that whitening the input of each layer (applying linear transformations to put the input with 0 mean and 1 covariance) helps to speed up training.
	
### Definition
1) When the input distribution to a learning system changes, it is said to *experience covariant shift*.
2) *Internal covariant shift* is when the input distribution of each layer changes.
	
	
# Method
## Dataset Normalization
For a layer with *d*-dimensional input, $x$, we normalize each feature over the entire training set:
$$\begin{equation}
\hat{x}^{(k)} = \frac{x^{(k)} - E[x^{(k)}]}{\sqrt{\text{Var}(x^{(k)})}}
\end{equation}$$
 
 The problem with normalizing inputs is that it can limit what they can represent. For instance,   the inputs of a sigmoid would constrain them to the linear regime of the non-linearity. To address this, the transformation inserted in the network will be able to represent the identity transform.
 
 For each activation, $x^{(k)}$, a pair of parameters $\gamma^{(k)}$, $\beta^{(k)}$, which scale and shift the normalized value:
$$\begin{equation}
y^{(k)} = \gamma^{(k)} x^{(k)} + \beta^{(k)}
\end{equation}$$

These parameters are learned along with the original model parameters, and restore the representation power of the network.  By setting $\gamma^{(k)} = \sqrt{\text{Var}[x^{(k)}]}$ and $\beta^{(k)}=E[x^{(k)}]$, we could recover the original activations, if that were the optimal thing to do.

## Batch Normalization
In stochastic gradient descent, mini-batches are used, so normalizations should take into account the batch and not the complete dataset.

Since the normalization is applied to each activation independently, let us focus on a particular activation $x^{(k)}$ and omit k for clarity. To calculate the batch normalization, for some mini-batch, we get the following algorithm:
![[algo1. Batch Normalization.png|400]]

## Inference Time
Once the network is trained, during test time, the variance and expected values used for normalization are the ones of the whole training data.

## Summary
Summing up the algorithm:
![[batchnormalization_algorithm2.png|400]]
**Note:** There are details in the paper about how to  adapt for convolution layers.

___
#General #Normalization