Paper: ***Empirical Evaluation of Gated Recurrent Neural Networks on Sequence Modeling***
*This paper did not introduce any of these concepts, just summarizes and compares them*
# Recurrent Neural Networks
A reurrent neural network, RNN, is a network that is reused, received the previous hidden state and an input at every time step.  There are several ariations, but the idea is to reuse the same parameters multiple times. Formally, given a sequence $x = (\mathbf{x_0}... \mathbf{x_t})$, a hidden state $\mathbf{h_t}$ can be recursively calcualted by
$$\begin{equation}
\mathbf{h_t}=\left\{
\begin{array}{ll}
      0 & t = 0\\
     \phi(\mathbf{h_{t-1}},\mathbf{x_t}) & \text{otherwise}
\end{array}, 
\right. 
\end{equation}$$

where $\phi$ is a nonlinear function.  Usually, this update is done in the following way:
$$\begin{equation}
\mathbf{h_t} = g(W\mathbf{x_t}+U\mathbf{h_{t-1}})
\end{equation}$$
where $g$ is a nonlinear activation function like the sigmoid function.

RNNs can also output a vector $\mathbf{y}$, where $y_t$ is returned at each timestep.

# Gating Units
Long RNNs sequences were hard to update mainly due to vainishing gradients (and sometimes exploding gradients). The problem was that the lsat hidden states had almost no information from the first ones, which made the gradients not reach there to conduct updates. \par

To respond to this, new, more complex, activation units, that retain past information more easily, have been created. These are called gating units.

## Long Short-Term Memory
A long short-term memorie, LSTM, is a complex activation unit that can be decomposed into a few vectors, some we call gates.  At each moment, there is an internal memory,  $\mathbf{c_t}$, that describes the complete stored state of the RNN iteration. This internal memory/state produces an output state, $\mathbf{h}$, which is visible to the *outside*:

$$ \begin{equation}
h^j_t = o^j_t \tanh(c^j_t)
\end{equation}$$
where $o^j_t$ is an output gate that is calculated using the previous hidden state, memory and current input,
$$\begin{equation}
o^j_t = \sigma(W_o\mathbf{x_t}+U_o\mathbf{h_{t-1}} + V_o \mathbf{c_t})^j
\end{equation}$$

The internal memory,  $\mathbf{c_t}$, is calculated using an internal memory candidate $\mathbf{\hat{c}_t}$ and the previous one:
$$ \begin{equation}
c^j_t = f^j_t c^j_{t-1} + i^j_t \hat{c}^j_t
\end{equation}$$

and the candidate is calculated by

$$ \begin{equation}
\hat{c}^j_t = \tanh(W_c\mathbf{x_t}+U_c\mathbf{h_{t-1}})^j
\end{equation}$$

The forget gate, $f$, mediates how much *old memory* stays and the input gate, $i$, mediates how much *new memory* is added. Both are given by:
$$ \begin{equation}
f^j_t = \sigma(W_f\mathbf{x_t}+ U_f\mathbf{h_{t-1}}+V_f\mathbf{c_{t-1}})^j 
\end{equation}$$

$$ \begin{equation}
i^j_t = \sigma(W_f\mathbf{x_t}+ U_f\mathbf{h_{t-1}}+V_f\mathbf{c_{t-1}})^j 
\end{equation}$$

**The $V$ matrices are diagonal matrices.**

![[Long Short-Term Memory.png]]

## Gated Recurrent Unit
Gated recurrent units, GRUs, are gating units that simplify the LSTMs. The main difference is that the hidden state output and the internal state (memory) are the same, as opposed to LSTMs.  This state is given by:

$$\begin{equation}
h^j_t = (1 - z^j_t) h^j_{t-1} + z^j_t \hat{h}^j_t
\end{equation}$$

the update gate, $z$, is given by
$$\begin{equation}
z^j_t = \sigma(W_z\mathbf{x}_t + U_z\mathbf{h}_{t-1})
\end{equation}$$


the candidate state $\hat{h}$  results from a forgetting gate, $r$,  the current input $\mathbf{x}_t$ and the previous state, $\mathbf{h}_{t-1}$:
$$\begin{equation}
\hat{h}^j_t = \tanh(W\mathbf{x_t}+U(\mathbf{r}_t \odot \mathbf{h_{t-1}}))^j 
\end{equation}$$

the reset gate is defines as
$$\begin{equation}
r^j_t = \sigma(W_r\mathbf{x}_t + U_r \mathbf{h}_{t-1})
\end{equation}$$

**The $V$ matrices are diagonal matrices.**


![[Gated Recurrent Unit.png]]

___
#General #RecurrentNetwork #GatingMechanism 


