Paper: [***Dream to Control: Learning Behaviors by Latent Imagination***](https://arxiv.org/pdf/1912.01603.pdf)


# Overview
It is a Model-Based Reinforcement Learning algorithm. The idea is to learn a latent state-space (like [[Muzero]]) and the use that to conduct upgrades without needing as much data. It does not use search, only a policy network. It also uses a value network, but only to conduct updates.
The paper is quite interesting as a whole.

The algorithm:
![[Pasted image 20220316213638.png]]


## World Model
$$\begin{align}
&\text{Representation model:} \qquad &p_\theta(s_t|s_{t-1},a_{t-1},o_t) \\
&\text{Transition model:} \qquad &q_\theta(s_t|s_{t-1},a_{t-1}) \\
&\text{Reward model:} \qquad &q_\theta(r_t|s_t) \\
\end{align}$$
## Action and Value Models
* Action Model:    $a_\tau \sim q_\phi(a_\tau|s_\tau)$
	  They sample the action not based on the probabilities, but directly using a neural network that outputs the action, so that the gradients can adjust directly the action to be chosen. They call this *parameterized sampling*:
	  $$\begin{equation}
	  a_\tau = \tanh(\mu_\phi(s_\tau) + \sigma_\phi(s_\tau)\epsilon)
	\end{equation}$$
	They are not too explicit, but I am assuming they divide the range of actions to equal intervals between 0 and 1.

* Value Model:   $v_\psi(s_\tau) \approx \mathbb{E}_{q(\cdot|s_\tau)} (\sum_{\tau=t}^{t+H} \gamma^{\tau-t} r_\tau)$
  To learn the action and value models, we need to estimate the state values of imagined trajectories $\{s_\tau , a_\tau , r_\tau\}_{r=t}^{t+H}$:
$$\begin{equation}
V_\lambda(s_\tau) = (1-\lambda)\sum_{n=1}^{H-1} \lambda^{n-1} V_N^n(s_\tau) + \lambda^{H-1} V_N^H(s_\tau)
\end{equation}$$
and 
$$\begin{equation}
V_N^k(s_\tau) = \mathbb{E}_{q_\theta,q_\phi}  (\sum_{n=\tau}^{h-1} \gamma^{n-\tau} r_n + \gamma^{h-\tau} v_\phi(s_h))  \ \ \ \ \ \text{with} \ \ \ \ h = \min(\tau + k, t + H)
\end{equation}$$

## Update Action and Value models.
The action model is used to navigate the environment, while the value model is used to update the former.
**To update the value model:**
$$\begin{equation}
\min_{\psi} \mathbb{E}_{q_\theta,q_\phi} (\sum_{r=t}^{t+H}||v_\psi(s_\tau)-V_\lambda (s_\tau)||^2
\end{equation}$$
**To update the action model:**
We update it in order to maximize $V_\lambda (s_\tau)$:
$$\begin{equation}
\max_\phi \mathbb{E}_{q_\theta,q_\phi} \sum_{\tau = t} V_\lambda (s_\tau)
\end{equation}$$
The value estimates depend on the reward and value predictions, which depend on the imagined states, which in turn depend on the **imagined actions**. Since all steps are implemented as neural networks, we try to improve this value directly by computing the gradient of this value estimation.

**The world model is fixed while learning behaviors.**


## Update the World Model
**Three different ways:**

* **Reward prediction.** The most basic way is to learn the rewards (jut like [[Muzero]]) and the states are automatically learned.
* **Reconstruction.** Used by [[PlaNet]]. Learns the dynamics by reconstructing images. The world model consists of the following components, where the observation model is only used to provide a learning signal:
$$\begin{align}
&\text{Representation model:} \qquad &p_\theta(s_t|s_{t-1},a_{t-1},o_t) \\
&\text{Observation model:} \qquad &q_\theta(o_t|s_t) \\
&\text{Reward model:} \qquad &q_\theta(r_t|s_t) \\
&\text{Transition model:} \qquad &q_\theta(s_t|s_{t-1},a_{t-1})
\end{align}$$
The observation model is used as an autoencoder:
![[Pasted image 20220316225416.png|250]]

*  **Contrastive estimation.** Predicting pixels can require high model capacity. We can also encourage mutual information between model states and observations by instead predicting the states from the images (Guo et al., 2018). This replaces the observation model with a state model,
  $$\begin{equation}
\text{State model:} \qquad q_\theta(s_t|o_t)
\end{equation}$$
We implement the state model as a CNN and again optimize the bound with respect to the combined parameter vector θ using stochastic backpropagation.
##### A comparison between these methods:
![[Pasted image 20220316230446.png|550]]

# Notes
The planning here is only to update the policy function and only the policy is used to make decisions, not the planning.This would be the equivalent as using the policy function in [[Muzero]] to decide the next action and only use the search tree to come up with an improved search tree policy.

Their criticism of [[Muzero]] is it requires large amounts of experience.
___
#ReinforcementLearning #ModelBasedReinforcementLearning #LatentStateSpace

