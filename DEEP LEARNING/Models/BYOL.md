Paper: [***Bootstrap Your Own Latent A New Approach to Self-Supervised Learning***](https://arxiv.org/pdf/2006.07733.pdf)

# Overview
- One of the most successful ideas thus far. 
- It does not use contrastive loss or clustering. 
- It prevents collapse by asymmetry in both learning and structure.
- Good intuitions.
- It uses a target network with older parameters.

## Image augmentations 
BYOL uses the same set of image augmentations as in SimCLR

# Method
The idea is to use Siamese networks, where there is an *online* and a *target* network (with parameters $\theta$ and $\xi$, respectively). The target network is sort of like an older version of the *online* network (like the target network in deep q-learning).

The following image summarizes the training pipeline:
![[Pasted image 20220206165130.png]]

1. An image is transformed into two different views, $v$ and $v'$, through different data augmentations.
2. These views are encoded into the representations $y_\theta$ and $y'_\xi$, through the functions $f_\theta(v)$ and $f_\xi(v')$.
3. The projector (only used for learning), transforms these representations into the projections $z_\theta$ and $z'_\xi$, through the functions $g_\theta(y_\theta)$ and $g_\xi(y'_\xi)$.
4. Lastly, **(only) the online network**, tries to use $z_\theta$ to predict $z'_\xi$
5. The prediction and $z'_\xi$ are normalized: $\overline{q_\theta}(z_\theta) = q_\theta(z_\theta)/||q_\theta(z_\theta)||_2$ and $\overline{z'_\xi} = z'_\xi / ||z'_\xi||_2$


## Loss 
The loss is given by the $\mathnormal{l}_2$ norm between the prediction and $z'_\xi$.

$$\begin{equation}
\mathcal{L}_{\theta,\xi} = ||\overline{q_\theta}(z_\theta)-\overline{z'_\xi}||^2_2 = 2 - 2\cdot \frac{q_\theta(z_\theta) \cdot z'_\xi}{||q_\theta(z_\theta)||_2 \cdot ||z'_\xi||_2}
\end{equation}$$


$v'$ is then passed through the target network to calculate the loss for this view $\mathcal{\tilde{L}}_{\theta,\xi}$. 

The BYOL loss is therefore:
$$\begin{equation}
\mathcal{L}_{\theta,\xi}^{\text{BYOL}} = \mathcal{L}_{\theta,\xi} + \mathcal{\tilde{L}}_{\theta,\xi}
\end{equation}$$

The update is as follows:
$$\begin{equation}
\theta \leftarrow \text{optimizer}(\theta,\nabla_\theta \mathcal{\tilde{L}}_{\theta,\xi},\eta)
\end{equation}$$

$$\begin{equation}
\xi \leftarrow \tau\xi + (1-\tau)\theta
\end{equation}$$

$\xi$ is therefore updated as an exponential moving average. 

## Inference Time
After training, only the encoder $f_\theta$ is kept.


# Intuition
This part is really interesting because it also gives insight on [[SimSiam]].

Their hypothesis is that the collapse would happen if you optimize the two parts of the Siamese network into a local minimum. By converging the two parts at the same time, they meet half-way into a collapse solution.

But, because BYOL does not use the target network to calculate the loss, the update is only made in one direction.

This is similar to [[Generative Adversarial Networks||GANs]], where there is no loss that is jointly minimized w.r.t. both the discriminator and generator parameters].


# Experiments
- ResNet-50

## Linear evaluation on ImageNet
![[Pasted image 20220206182824.png]]

## Semi-supervised training on ImageNet
![[Pasted image 20220206183001.png]]

## Transfer to other classification tasks
![[Pasted image 20220206183018.png]]

# Ablations
## Batch size
![[Pasted image 20220206183132.png|300]]

## Transformations
![[Pasted image 20220206183147.png|300]]

## Target Network update temperature
![[Pasted image 20220206185110.png|300]]

## Predictor Network
![[Pasted image 20220206185349.png|300]]


# Summary
![[Pasted image 20220206185413.png]]

___
#SelfSupervisedLearning 