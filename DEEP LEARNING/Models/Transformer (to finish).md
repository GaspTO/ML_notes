Paper: [***Attention Is All You Need***](https://arxiv.org/pdf/1706.03762.pdf)

Resource: [Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)

# Overview
Encoder-decoder/seq2seq mechanisms use attention mechanisms that relate the importance of each part of the output to the different parts of the input.   The self-attention mechanism does something different. It relates each part of the input (or output) to the other parts of the input (or output, respectively). 
![[Self-attention1.png|500]]
<br>

There are different types of attention. The one used here is called *scaled dot-product attention*. This one uses three vectors: queries, $Q$, keys,$K$, and values,$V$.
$$\begin{equation}
\text{Attention}(Q,K,V) = \text{softmax}(\frac{QK^\intercal}{\sqrt(d_k)}) V
\end{equation}$$
The input is multiplied by matrices that compute the Q,K and V; and only then the scaled dot-product attention is calculated using them.
![[Matrix scaled dot-product attention 1.png|200]] ![[Matrix scaled dot-product attention 2.png|200]]

The scaling factor, $d_k$, is because dot products, if they have a lot of terms, can achieve huge values that put the softmax in regions with low gradients (at least that's the paper's hypothesis).
<br>

**This operation is done for multiple heads.** There multiple $W_Q$,$W_K$ and $W_V$ matrices.  And the self-attention is calculated in parallel
![[Multi-head attention1.png|400]] ![[Multi-headed attention2.png|200]]
<br>

After they are done, all the resulting vectors of each attention are concatenated and they are multiplied by another matrix, $W_O$, that shrinks it
![[Multi-headed attention3.png|400]]


# Transformer
The whole architecture uses this attention mechanism but it is a bit more complicated.
![[Transformer architecture.png|300]]
<br>

(to finish)

___
#General #NaturalLanguageProcessing #Architecture  #Transformer #Attention 


