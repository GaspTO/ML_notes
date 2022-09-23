Paper: [Self-Supervised Learning of Pretext-Invariant Representations](https://arxiv.org/pdf/1912.01991.pdf)

Honestly, this paper is so ridiculously written...


# Overview
Self-Supervised Learning with Contrastive loss.

![[Pasted image 20220202194835.png|400]]

![[Pasted image 20220202195611.png|400]]


Trains representations to be invariant to data augmentation instead of simply covariant.


## Simple Loss
The **Loss**, for an image $\mathbf{I}$ and it's transformation $\mathbf{I}^t$, is:


where $L_{\text{NCE}}$ is
$$\begin{equation}
L_{\text{NCE}}(\mathbf{I},\mathbf{I}^t) = -\log [h\big(f(\mathbf{v_I}),g(\mathbf{v_I^t})\big)] - \sum_{I' \in \mathcal{D}_N }\log [1-h\big(g(\mathbf{v_I^t}),f(\mathbf{v_I'})\big)]
\end{equation}$$


where $h$ is given by:
$$\begin{equation}
h(\mathbf{v_I},\mathbf{v_I^t}) = \frac
{\exp(\frac{s(\mathbf{v_I},\mathbf{v_I^t})}{\tau})}
{\exp(\frac{s(\mathbf{v_I},\mathbf{v_I^t})}{\tau}) + \sum_{I'\in \mathcal{D}_N} \exp(\frac{s(\mathbf{v_I^t,v_I'})}{\tau})}
\end{equation} \tag{4}$$



## Memory Bank
The problem is that iterating over the entire dataset is really expensive and using a large number of negative examples is important.  In a mini-batch SGD optimizer, it is difficult to obtain a large number of negatives without increasing the batch to an infeasibly large size. To address this problem, they use a memory bank of “cached” features.

The memory bank, $M$, contains a feature representation $\mathbf{m_I}$ for each image $\mathbf{I}$ in dataset $\mathcal{D}$. The representation mI is an exponential moving average of feature representations $f(\mathbf{v_I})$ that were computed in prior epochs.

So, in equation 4, we now replace $f(\mathbf{v_I'})$  by $\mathbf{m_I'}$.

All computed on the original images,  $\mathbf{I}$, without the transformation $t$.


## Complex Loss
A potential issue of the loss in Equation 4 is that it does not compare the representations of untransformed images I and I 0 . We address this issue by using a convex combination of two NCE loss functions:

$$\begin{equation}
L(\mathbf{I},\mathbf{I}^t) = \lambda L_{\text{NCE}}(\mathbf{m_{I}},g(\mathbf{v_{I^t}})) + (1-\lambda) L_{\text{NCE}}(\mathbf{m_{I}},f(\mathbf{v_{I}}))
\end{equation}$$

**The first term** is simply the loss of Equation 4 but uses memory representations $\mathbf{m_I}$ and $\mathbf{m_I'}$ instead of $f(\mathbf{v_I})$ and $f(\mathbf{v_I'})$, respectively.

**The second term** uses only $\mathbf{m_I'}$ instead of $f(\mathbf{v_I'})$.







___
#SelfSupervisedLearning  #ContrastiveLearning  