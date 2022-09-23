## Overview
They use RSA to analyses models and brains (see this [topic](obsidian://open?vault=Preluna&file=Computational%20Neuroscience%2FTopics%2FSimiliary%20matrices%20and%20Analysis)). From what I understand, they use all the neurons of each model, but I might be wrong.


They compared 27 not-strongly supervised models with the human IT and monkey IT and non of them was really that good. The best one was one formed by the combination of those 27. This combination happened by summarizing each model by their first 95 principal components.

Here's the RDMs of the 7 models that explain the monkey IT the most
![[Pasted image 20220412155134.png]]

And the same for the human IT
![[Pasted image 20220412155201.png]]

So far, we showed that none of the not-strongly-supervised models were able to reproduce the categorical structure present in IT. Most of these models were untrained or trained without supervision. A few of them were weakly supervised (i.e. supervised with merely 884 training images). Their failure at explaining our IT data suggests that computational features trained to cluster the categories through supervised learning with many labeled images might be needed to explain the IT representational geometry. 

We therefore tested a deep convolutional neural network trained with 1.2 million labelled images, nonoverlapping with the set of 96 images used here. The model has eight layers. The RDM for each of the layers and the RDM correlations with hIT and mIT are
shown in the next figure:

![[Pasted image 20220412164713.png]]

Here's the similarity between the layer RDMs and human IT RDM:
![[Pasted image 20220412164817.png|400]]


# Data
We used the experimental stimuli from Kriegeskorte et al. 
The stimuli were 96 images which half were animates and the other half were inanimates. The animate cluster consisted of faces and bodies, and the inanimate cluster consisted of natural and artificial inanimates.