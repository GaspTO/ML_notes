Paper: ***Neural Turing Machines***

# Overview
A **Neural Turing Machine** (NTM) is composed by two parts:
1.	Memory
2.	Controller

The following image is the architecture of a NMT

![[Neural Turing Machine Architecture.png|300]]


The controller interacts with the external world through input and output vectors. 

Transparently to the outside, the controller interacts with the memory through **read** and **write** operations.  The outputs of the controller network that parameterise these operations are called **heads**.

The whole architecture is differentiable, which is achieved by having the heads cover all the memory. 


Let  $M_t$ be Memory with dimensions $N\text{x}M$  ($N$ rows, $M$ vector size).
# Reading Operation
Let  $\mathbf{w}_t$ be a vector over the $N$ rows emmited by the reading head. The vector is normalized: $\sum_{i} w_t(i) = 1$.

The read vector is given by
$$\begin{equation}
\mathbf{r}_t \leftarrow \sum_{i} w_t(i) \mathbf{M}_t(i)
\end{equation}$$


# Writing Operation
The Writing head has two different parts: *erasing* and *adding*.

Let
1. $\mathbf{w}_t$ be a normalized vector over the $N$ rows emmited by the writing head. 
2. the erase vector, $\mathbf{e}_t$, be a normalized vector with size $M$.
3. the add vector, $\mathbf{a}_t$, be a normalized vector with size $M$.

**The previous memory $\mathbf{M}_{t-1}$ can be erased as following:**
$$\begin{equation}
	\hat{M}_t(i) \leftarrow M_{t-1}(i)[\mathbf{1}-w_t(i)e_t]
\end{equation}$$
Where $\mathbf{1}$ is a row-vector with all-1s. Therefore, a memory location is completely erased if $e_t$ and $w_t(i)$ are set to all-1s.

**The previous memory $\mathbf{M}_{t-1}$ can be added as following:**
After the erasing operation:
$$\begin{equation}
	M_t(i) \leftarrow \hat{M_t}(i) + w_t(i)\mathbf{a}_t
\end{equation}$$

(** NOTE: **  the erase and add vectors are independent)


# Addressing Operation
In the previous operations, we assumed a given weighted vector, $\mathbf{w}_t$. Now, we describe how these are produced.

They are the result of two different types of addressing: *content-based* and *location-based* addressing.  **Content-based addressing** finds the locations in memory that are similar to some value. **Location-based addressing** finds the location in memory identified by some value that represents a location Read the paper to know the advantages and disadvantages of each.

## Content-based addressing 
It is similar to the first attention mechanism proposed (todo: put citation).  The respective (either write or read) head produces a key vector $\mathbf{k}_t$ that is supposed to be an approximation of the true stored value in memory.  The key is then compared to each vector in memory using some measure of similarity, given by a function $K$:
$$\begin{equation}
w_t^c(i)\leftarrow \frac{\exp(\beta_t K[\mathbf{k}_t,\mathbf{M}_t(i)])}{\sum_{j}\exp(\beta_t K[\mathbf{k}_t,\mathbf{M}_t(j)])}
\end{equation}$$
Where $\beta_t$ is a constant that mediates the similarity precision. An example of a similarity measure is the cosine similariy:
$$\begin{equation}
K[u,v] = \frac{\mathbf{u}\cdot\mathbf{v}}{||\mathbf{u}||\cdot||\mathbf{v}||}
\end{equation}$$

After the content-based addressing weight vector is calculated, it is added, through a gating value $\mathbf{g}_t \in [0,1]$ to the previous weight vector:
$$\begin{equation}
\mathbf{w}_t^g \leftarrow g_t \mathbf{w}_t^c + (1-g_t)\mathbf{w}_{t-1}
\end{equation}$$

## Location-based addressing 
The location-based addressing is done after the content-based addressing (remember, a NTM uses a mix of both). It is based on the idea of memory shifts. The simplest way to decide on the shift is to decide a shift range (e.g. -1 to 1) and get a shift vector, $\mathbf{s}_t$, using a softmax. 

For every memory location, the new, shifted weight vector is given by
$$\begin{equation}
\hat{w}_t(i) \leftarrow \sum_{j=0}^{N-1} w_t^g(j)s_t(i-j)
\end{equation}$$
It's a convolution operation. 
But this can create *blurred* shifts (read paper). So, finally, we do:
$$\begin{equation}
w_t(i) \leftarrow \frac{\hat{w}_t(i)^{\gamma_t}}{\sum_j \hat{w}_t(j)^{\gamma_t}}
\end{equation}$$
with $\gamma_t \ge 1$.


<br> </br>
Finally, getting $\mathbf{w}_t$.  The combination of both of these addressings is good because the backpropagation can always choose one or blend them. In the latter case, by using content addressing, it can find a block, and then access a particular element inside of it.

The following figure summarizes the addressing mechanism:
![[Neural Turing Machine Adressing Mechanism.png]]

## Controller network
There are certain free parameters to choose: *memory size*, *number of read and write heads*, *range of location shifts*. And, obviously, the type of network architecture (read the paper for more information).

___
#Vision #Architecture #Memory #GatingMechanism 























