Paper: [***Residual Networks Behave Like Ensembles of Relatively Shallow Networks***](https://arxiv.org/pdf/1605.06431.pdf)

# Overview
This paper shows that the reason [[ResNet|ResNets]] work is because they behave like an ensemble of shorter networks.

We can look at a network in an unrolled way:
![[Pasted image 20220122020550.png]]
Each dot is an output. There is $y_0$, $y_1$, $y_2$ and $y_3$ here. We can count 8 paths.
Or, in an algebric way:
$$\begin{split}
& y_3 = y_2 + f_3(y_2) \Leftrightarrow  \\
& [y_1 + f_2(y_1)] + f_3(y_1 + f_2(y_1)) \Leftrightarrow \\
& [y_0 + f_1(y_0) + f_2(y_0 + f_1(y_0))] + f_3(y_0 + f_1(y_0) + f_2(y_0 + f_1(y_0)))
\end{split}$$

We can then analyse the paths when we disable a layer:
![[Pasted image 20220122023041.png]]

**Questions asked after this formulation:**
- Are the paths in residual networks dependent on each other or do they exhibit a degree of redundancy?
- If the paths do not strongly depend on each other, do they behave like an ensemble? 
- Do paths of varying lengths impact the network differently


# Lesion Study
In this section, we use three lesion studies to show that paths in residual networks do not strongly depend on each other and that they behave like an ensemble.


## Experiment: Deleting individual layers from neural networks at test time
next figure
![[Pasted image 20220122023335.png]]

##  Experiment: Deleting many modules from residual networks at test-time
next figure 5(a)
![[Pasted image 20220122023512.png]]

## Experiment: Reordering modules in residual networks at test-time
previous figure 5(b)


# The importance of short paths in residual networks
Finally, we can use these results to deduce whether shorter or longer paths contribute most of the gradient during training.
Surprisingly, almost all of the gradient updates during training come from paths between 5 and 17 modules long. These are the effective paths, even though they constitute only 0.45% of all paths through this network. Moreover, in comparison to the total length of the network, the effective paths are relatively shallow.
![[Pasted image 20220122023632.png]]


To validate this result, they retrained a residual network from scratch that only sees the effective paths during training. This ensures that no long path is ever used. Their results were pretty much the same as the baseline.


# Further discussion
## Highway network
Connection to highway networks In [[Highway Networks|highway networks]], $t_i(·)$ multiplexes data flow through the residual and skip connections and $t_i(·) = 0.5$ means both paths are used equally. For highway networks in the wild, [19] observe empirically that the gates commonly deviate from $t_i(·) = 0.5$. In particular, they tend to be biased toward sending data through the skip connection; in other words, the network learns to use short paths. Similar to our results, it reinforces the importance of short paths

## Stochastic depth
[[Stochastic Depth|Stochastic Depth]] changes the distribution of paths seen during training. In particular, mainly short paths are seen. Further, by selecting a different subset of short paths in each mini-batch, it encourages the paths to produce good results independently.
Does this training procedure significantly reduce the dependence between paths? We repeat the experiment of deleting individual modules for a residual network trained using stochastic depth. The result is shown in Figure 8. Training with stochastic depth improves resilience slightly; only the dependence on the downsampling layers seems to be reduced. By now, this is not surprising: we know that plain residual networks already don’t depend on individual layers.
![[Pasted image 20220122030024.png|400]]

___
#ArchitectureAnalysis #Architecture #ResidualConnection 