Paper: ***Highway Networks***

# Overview
Have blocks:
$$\begin{equation}
\mathbf{y} = H(\mathbf{x},\mathbf{W_H}) \cdot t(\mathbf{x},\mathbf{W_T}) + \mathbf{x} \cdot C(\mathbf{x},\mathbf{W_c})
\end{equation}$$

$T$ is called transformation gate and $C$ carry gate.

This paper uses 
$$\begin{equation}
C = 1 - T
\end{equation}$$

Which makes the previous block
$$\begin{equation}
\mathbf{y} = H(\mathbf{x},\mathbf{W_H}) \cdot t(\mathbf{x},\mathbf{W_T}) + \mathbf{x} \cdot (1-T(\mathbf{x},\mathbf{W_T}))
\end{equation}$$

# Experiments
![[Highway network experiments.png]]


___
#General #Vision #Architecture #ConvolutionalNetwork #GatingMechanism #ResidualConnection 
