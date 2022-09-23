It's probably the best paper (I've read so far) to understand the whole context of the neuroscientific understandment of the brain using deep learning models.
It has these boxes (that I will put in this post) that are like important *side notes* , specific technical understandment about this area.

# Overview
The paper supports the idea that we shoud aim to create models that are good at behavioural tasks because, as a consequence, they seem to be the best ones at predicting the brain, as it shows in the next figure. The Brainsc
![[Pasted image 20220405142712.png|400]]


# Summaries
The papers has these summary boxes, which are quite useful to understand some of the main components of this are and what is pretended from it. 

## Minimal criteria for a sensory enconding model
A sensory enconding model that attempts to model a sensory cortical system needs to have 3 properties:
* **Stimulus-computability:** The model should accept arbitrary stimuli within the general stimulus domain of interest.
* **Mappability:** The components of the model should correspond to experimentally definable components of the neural system. 
* **Predictivity:** The units of the model should provide detailed predictions of stimulus-by-stimulus responses, for arbitrarily chosen neurons in each mapped area.

Although these criteria are ideal, in practice they might conflict with each other.

## Mapping models to neural sensory systems
How does one map artificial neural networks to real neurons? (In the same order as in the paper, but with different names):
1. **Task Information consistency.** At the coarsest level, a useful metric of model similarity to a system is the consistency of patterns of explicitly decoda-ble information available to support potential behavioral tasks. In this approach, populations of ‘neurons’ from a model and populations of recorded neurons are analyzed with identical decoding methods on a battery of high-level tasks (for example, object recognition, face identification and so forth). While not required, it is useful to use simple decoders such as linear classifiers or linear regressors, as these embody hypothetical downstream decoding circuits. This procedure generates a pattern of response choices for both the model and the neural population. (for example, via accuracy levels for each task 32 ) or a fine grain (stimulus-by-stimulus response consistency). We note that this approach naturally connects to the linkage between neuronal populations and behavior. (I haven't understood this completely, but I think the idea is to go from neuronal response to output and train a classifier for both model and real neurons and then compare them).
2. **Similarity matrices.** The two representations (real neurons and that of the model) are characterized by their pairwise stimulus correlation matrix (next figure): for a given set of stimuli, this matrix describes how far apart a representation ‘thinks’ each pair of stimuli are.  ![[Pasted image 20220405133204.png|400]] These distance matrices are then compared for similarity: the model is judged to be similar to the neural representation if it treats stimuli pairs as close to (or far from) each other whenever the real neural population representation also does so.  
4. **Target neurons predictivity**: A *finer grained* mapping of models to neurons is that of linear neural response predictivity of single units. How would one map the neurons in the source to neurons in the target? In many brain areas (such as, for example, V4 or IT), there might not be an exact one-to-one mapping of units between the animals. However, it is reasonable to suppose that the two animals’ areas are the same (or very similar) up to linear transform - for example, that units in the target animal are aproximately linear combinations of (a small number of) units in the source animal. In engineering terms, the animals would be said to be ‘equivalent bases’ for sensory representation. The goal is find linear combinations of model units that together produce a *synthetic neuron* that will reliably have the same response patterns as the original target real neuron: find $c_i \in \{1, ..., n\}$ such that 
	$$\begin{equation}
	r(\mathbf{x}) \approx r_{\text{synth}}(\mathbf{x}) = \sum_i c_i 
	\ m_i(\mathbf{x})
\end{equation}$$where $r(\mathbf{x})$ is the response of neuron $r$ to stimulus $\mathbf{x}$, and $m_i(\mathbf{x})$ is the response of the i-th model unit (in some fixed model layer).
	

## The meaning of ‘understanding’ in a complex sensory system
have suggested that successful models are image-computable, mappable and quantitatively predictive. But do models that meet these criteria necessarily represent understanding? It can be argued that deep neural networks are black boxes that give limited conceptual insight into the neural systems they aim to explain. Indeed, the very fact that deep CNNs are able to predict the internal responses of a highly complex system performing a very nonlinear task suggests that, unlike earlier toy models, these deeper models will be more difficult to analyze than earlier models. **There may be a natural tradeoff between model correctness and understandability.**

However, one of the key advantages of an image-computable model is that it can be analyzed in detail at low cost, making high-throughput ‘virtual electrophysiology’ possible.

Unlike efficient coding, goal-driven HCNNs derive their objective function from behaviors that organisms are known to perform, rather than more abstract concepts, such as sparsity, whose ecological relevance is unclear. Investigating how a system’s computational-level goalsinfluence its algorithmic and implementation level mechanisms is also related to neuroethology, where the natural behavior of an organism is studied to gain insight into underlying neural mechanisms.