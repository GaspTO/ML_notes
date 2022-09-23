Paper: ***Deep Residual Learning for Image Recognition***

# Overview
It allowed for the first time for networks with hundreds of layers in depth to be trained. It uses redidual connections, seen in the following figure:
![[Residual Connection.png]]

One of the motivations comes from the fact that adding layers to a good network can make it worse. This should not happen since, at the very worst, the extra layers should simply learn the identity function. From this it can be concluded that learning the identity function might not be so *natural* or easy.

So, the idea of using residual networks is such that each layer learns a *residue*:
$$\begin{equation}
F(x) = H(x) - x
\end{equation}$$
if $F(x)$ is the learned function and $H(x)$ is the optimal desired function to learn.

To the extreme, if an identity mapping were optimal, it would be easier to push the residual to zero than to fit an identity mapping by a stack of nonlinear layers.


# Identity Mapping by Shortcuts
A block is
$$\begin{equation}
\mathbf{y} = \mathcal{F}(\mathbf{x},W_i) + \mathbf{x}
\end{equation}$$
For the example in Fig. 2 that has two layers, $\mathcal{F} = W_2 \cdot \sigma(W_1 \cdot \mathbf{x})$ in which $\sigma$ denotes a ReLU.

These shortcut connections do not introduce any complexity, since there are no parameters. 

The layer dimensions might not be the same, so we need to project linearly $\mathbf{x}$ into a shorter space. 
$$\begin{equation}
\mathbf{y} = \mathcal{F}(\mathbf{x},W_i) + W_s\mathbf{x}
\end{equation}$$
$W_s$ is learned. This matrix can be used in the previous equation, but their experiments showed it was not necessary (and it is more economical).


If $\mathcal{F}$ is a block with only one layer then
$$\begin{equation}
\mathbf{y} = W_i\mathbf{x}+ \mathbf{x}
\end{equation}$$
But it can be any other functions.

# Experiments
There's  two types of building blocks shown.
![[Pasted image 20211231161317.png|400]]
These were the ones used. The bottleneck architectures is simply to save computation.  The first layer servers to decrease layers, the second is the proper computation and the third to expand.
![[Pasted image 20211231161600.png]]

Two types of networks were 	compared: *plain* and *residuals*. They are the same, but one has residual connections and the other does not.  

When two plains of different sizes were compared:
![[Resnet  plain net 18vs34 layer.png|300]]

When two resnets of different sizes were compared:
![[Resnet 18vs34.png|300]]

The degradation problem talked earlier seems to happen for plain networks, but not for resnets.
The 18-plain and 18-resnet are comparable, it only seems to make difference when the network is too deep.

The degredation problem does not seem to exist with residual networks, so the best network is a 152 layer deep resnet
![[Resnet comparison table.png|300]]

Finally, They combine six models of different depth to form an ensemble (only with two 152-layer ones at the time of submitting). This leads to 3.57% top-5 error on the test set. This entry won the 1st place in ILSVRC 2015.

# Notes
The argument made in this paper is that, to train very deep networks, residual connections are needs. In the inception-v4 paper, they argue that it is not necessary, but the residual connections speed up the training a lot.

# Questions
I wonder if the residual connections facilitate because it creates shorter paths, where the gradient doesn't vanish; in other words, it is using shallower layers more instead of deeper ones, or if it facilitates because learning the residue of a function is simpler. A way of testing this is by not allowing the gradients to propagate through residual connections.

___
#General #Vision  #ConvolutionalNetwork  #ResidualConnection #Architecture #Layer


