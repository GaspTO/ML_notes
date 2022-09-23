Paper: [***Meta Pseudo Labels***](https://arxiv.org/pdf/2003.10580.pdf)

# Overview
This paper changes the teacher at the same time it changes the student.
![[MPLfigure1.png]]


Pseudo Labels as an optimization problem. To introduce Meta Pseudo Labels, let’s first review Pseudo Labels. Specifically, Pseudo Labels (PL) trains the student model to minimize the cross-entropy loss on unlabeled data:
$$\begin{equation}
\theta^{PL}_S \leftarrow \arg \min_{\theta_{S}} \bigg(\mathcal{L}_u(\theta_T,\theta_S) = \mathbb{E}_{x_u}[\text{CE}(T(x_u;\theta_T),S(x_u;\theta_S))] \bigg)
\end{equation} \tag{1} $$
$\textbf{CE}$ is cross entropy.

The pseudo target $T(x_u; \theta_T)$ is produced by a well pre-trained teacher model with fixed parameter $\theta_T$ . Given a good teacher, the hope of Pseudo Labels is that the obtained $\theta_S^{PL}$ would ultimately achieve a low loss on labeled data,
$$\begin{equation}
\mathcal{L}_l(\theta_S^{PL}) = \mathbb{E}_{x_l,y_l}[\text{CE}(y_l,S(x_l,\theta_S^{PL}))]
\end{equation} \tag{2} $$

$\theta_S^{PL}$  depends therefore on the parameters of the teacher, so we can write $\mathcal{L}_l(\theta_S^{PL}(\theta_T))$.

So, we could also try to minimize this loss as a function of the teacher

$$\begin{align}
\min_{\theta_T}& \ \ \ \ \mathcal{L}_l(\theta_S^{PL}(\theta_T)) \\
\text{where}& \ \ \ \ \theta_S^{PL}(\theta_T) = \arg \min_{\theta_S} \mathcal{L}_u(\theta_T,\theta_S)
\end{align} \tag{3} $$

Intuitively, by optimizing the teacher’s parameter according to the performance of the student on labeled data, the pseudo labels can be adjusted accordingly to further improve student’s performance.

However, the dependency of $\theta^{PL}_S(\theta_T )$ on $\theta_T$ is extremely complicated, as computing the gradient $\nabla \theta^{PL}_S(\theta_T)$ requires unrolling the entire student training process (i.e. $\arg \min_{\theta_S}$)

**Practical approximation.** $\theta^{PL}_S(\theta_T) \approx \theta_S - \eta_S \cdot \nabla_{\theta_S} \mathcal{L}_u(\theta_T,\theta_S)$, where $\eta_S$ is the learning rate. Plugging this approximation into the optimization problem in the equation  $(3)$ leads to the practical teacher objective in Meta Pseudo:
$$\begin{align}
\min_{\theta_T}& \ \ \ \ \mathcal{L}_l(\theta_S^{PL}-\eta_S \cdot \nabla_{\theta_S} \mathcal{L}_u(\theta_T,\theta_S)) \\
\text{where}& \ \ \ \ \theta_S^{PL}(\theta_T) = \arg \min_{\theta_S} \mathcal{L}_u(\theta_T,\theta_S)
\end{align} \tag{4a} $$


if soft pseudo labels are used, i.e. $T(x_u; \theta_T)$ is the full distribution predicted by teacher, the objective above is fully differentiable with respect to $\theta_T$ and we can perform standard back-propagation to get the gradient (when optimizing this equation, we always treat $\theta_S$ as fixed parameters and ignore its higher-order dependency on $\theta_T$). 

However, in this work, we sample the hard pseudo labels from the teacher distribution to train the student. We use hard pseudo labels because they result in smaller computational graphs.

the student’s parameter update can be reused in the one-step approximation of the teacher’s objective, which naturally gives rise to an alternating optimization procedure between the student update and the teacher update:

1. **Student:** draw a batch of unlabeled data $x_u$, then sample $T(x_u;\theta_T)$ from teacher's prediction, and optimize equation $(1)$

2. **Teacher:** draw a batch of labeled data $(x_l , y_l)$, and *reuse* the student's update to optimize objective $(3)$ with SGD: $\theta'_T = \theta_T - \eta_T \nabla_{\theta_T} \mathcal{L}_l (\theta_S - \nabla_{\theta_S}\mathcal{L}_u(\theta_T,\theta_S))$. See below the implementation details to know exactly how this is done.

#  Auxiliary Goals
They empirically observe that Meta Pseudo Labels works well on its own (unlike the pseudo labels where the teacher is fully trained with supervised learning beforehand), but they use augment the training with a semi-supervised and a supervised objective.
- For the supervised objective, we train the teacher on labeled data.
- For the semi-supervised objective, we additionally train the teacher on unlabeled data using the [[UDA]] objective.

# Pseudo Code
![[MPLalgorithm1.png]]
	


# Implementation Details
It is not straightforward how to use the following formula in practice,
$$\begin{equation}
\min_{\theta_T} \ \ \mathcal{L}_l(\theta_S-\eta_S \cdot \nabla_{\theta_S} \mathcal{L}_u(\theta_T,\theta_S))
\end{equation} \tag{4b} $$
The appendix of the paper redefines the past formula into something more usable (See the paper's appendix for the full derivation):
$$\begin{align}
\nabla_{\theta_T}\mathcal{L}_l &= 
\eta_S \cdot \bigg(
\big(\nabla_{\theta'_S}\text{CE}(y_l,S(x_l;\theta'_S))\big)^\intercal
\cdot \nabla_{\theta_S}\text{CE}(\hat{y_u},S(x_u;\theta_S))
\bigg) \cdot \nabla_{\theta_T}\text{CE}(\hat{y_u},T(x_u;\theta_T)) = \\
&= h \cdot \nabla_{\theta_T}\text{CE}(\hat{y_u},T(x_u;\theta_T)) \end{align} \tag{5} $$

The $h$ can be seen as a scalar since it does not depend on the teacher's parameters, $\theta_T$.
However, this can still be upsetting to calculate. In this [paper's google research repository](https://github.com/google-research/google-research/tree/ec13eb6661a7b9500016cc6d7e3ab940c2dbf184/meta_pseudo_labels), and in the other repositories I've seen, they approximate $h$ by
$$\begin{equation}
h = \text{CE}(y_l,S(x_l;\theta'_S)) - \text{CE}(y_l,S(x_l;\theta_S))
\end{equation} \tag{6} $$ 
In other words, $h$ is estimated using the difference between the cross-entropy of the labeled data before and after then student update. The explaination I will write can be seen in [here](https://github.com/google-research/google-research/issues/534).
<br>
<br>

## Explanation
This expression results from unrolling the equation $(5)$ for 2 terms (linear approximation) using the taylor expansion around the point $\theta_S$.  Remember this expression:
$$\begin{equation}
f(x) = f(a) + \nabla f(a)(x-a)
\end{equation} \tag{7} $$
If we define $v = x - a$, we can write
$$\begin{align}
f(a + v) = f(a) + \nabla f(a)v \
\end{align} \tag{8} $$
1. Define
$$\begin{equation}
a = \theta_S
\end{equation} \tag{9} $$
2. Define
$$\begin{equation}v = \theta_S' - \theta_S 
\end{equation} \tag{10} $$
3. Given the update rule
 $$\begin{equation} \theta_S' = \theta_S - \eta_S \nabla_{\theta_S}\text{CE}(\hat{y_u},S(x_u;\theta_S))\end{equation} \tag{11} $$
4. Therefore 
$$\begin{equation}v = \theta_S' - \theta_S = - \eta_S \nabla_{\theta_S}\text{CE}(\hat{y_u},S(x_u;\theta_S))
\end{equation} \tag{12} $$
And
$$\begin{align}
&f(a + v) - f(a) = \nabla f(a){v} \ \Leftrightarrow \\ \\
\Leftrightarrow \ &f(\theta_S') - f(\theta_S) = -\eta_S \nabla f(\theta_S) \cdot { \nabla_{\theta_S} \text{CE}(\hat{y_u},S(x_u;\theta_S))} \ \Leftrightarrow \\ \\ 
\Leftrightarrow \ &f(\theta_S) - f(\theta_S') = \eta_S \nabla f(\theta_S) \cdot { \nabla_{\theta_S} \text{CE}(\hat{y_u},S(x_u;\theta_S))}
\end{align} \tag{13} $$

5. If $f$ is the cross entropy of the labeled set, we finally have
$$\begin{align}
h = \text{CE}(x_l,\theta_S) - \text{CE}(x_l,\theta_S') = \eta_S \nabla \text{CE}(x_l,\theta_S) \cdot { \nabla_{\theta_S} \text{CE}(\hat{y_u},S(x_u;\theta_S))}
\end{align} \tag{14} $$

(This gave the symmetric. In one pytorch implementation, the symmetric is used. I am not sure if I made a mistake. if it is indifferent or they got it wrong).


___
#General #Vision  #ConvolutionalNetwork #Distillation #TransferLearning
