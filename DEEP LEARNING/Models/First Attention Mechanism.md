Paper: ***Neural Machine Translation by jointly learning to align and translate***


# Overview 
The first attention mechanism was described in this paper and it was used for translation.
<br>
There is a phrase to be translated given by a sequence of  word vectors $\mathbf{x} = (x_1,...,x_T)$.  The objective is to find the probability of a sequence of word vectors, that correspondends to a possible translation, $p(\mathbf{y}|\mathbf{x}) = \prod^T_{t=1} p(y_t|y1...y_{t-1},\mathbf{x})$. The idea is to use a RNN to,  in each step, given all the previous chosen translated words $y_1...y_{t-1}$, estimate the probability of lots of other possible new words and choose for $y_{t}$ the one that maximizes it. 


The proposed architecture defines:
$$\begin{equation}
p(y_i|y_1,...,y_{i-1},\mathbf{x}) = g(y_{i-1},s_i,c_i)
\end{equation}$$  
where $c_i$ is a context vector, $g$ is a neural network and $s_i$ is a hidden vector calculated by
$$\begin{equation}
s_i = f(s_{i-1},y_{i-1},c_i)
\end{equation}$$  

The context vector, $c_i$, holds information about the input, $\mathbf{x}$, and it is different in each time step. This vector will result from *attention* given to annotation vectors, $h_i$, which are calculated only using the input and are supposed to encode information about it.
$$\begin{equation}
c_i = \sum^T_{j=1}\alpha_{ij}h_j
\end{equation}$$  
where the $\alpha_{ij}$  mediates the *attention* given to each annotation vector and they sum to $1$.  They are given by
$$\begin{equation}
\alpha_{ij} = \frac{\exp(e_{ij})}{\sum^T_{k=1}\exp(e_{ik})}
\end{equation}$$  
and $e_ij$ is defined by
$$\begin{equation}
e_{ij} = a(s_{i-j},h_j)
\end{equation}$$

<br>
<br>

**But how are annotation vectors, $h_t$, calculated?**
They are the concatenation of a forward annotation vector, $\overrightarrow{h_t}$, and a backwards one ,$\overleftarrow{h_t}$.

The forward one is accomplished by passing a RNN from word $x_1$ until $x_T$. In each word as input, there is a hidden state outputted which  is the forward annotation vector.  For the backwards one, the same process is done using a RNN but now starting at $x_T$ and ending in $x_1$.

___
#General #RecurrentNetwork #Translation #NaturalLanguageProcessing #Attention
