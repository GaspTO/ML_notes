# Problem
**Question 1:** under what training conditions would specialized ventral-like and dorsal-like pathways emerge in a deep ANN?

**Question 2:** Would a second loss function be required to obtain matches to dorsal pathways, or is there a single loss function that could induce both types of representations?

# Experiments
They use a self-supervised technique called contrastive predictive coding (CPC) and explore two networks - ResNet backbones, one with only one pathway and the other that divides itself into two after the first convolution.

The baselines are (1) a simple model based on Gabor filters, (2) a randomized deep ANN, (3) a ResNet-18 trained on ImageNet, and (4) 3D ResNets trained on action recognition in a supervised manner.

# Results
1. CPC with a single pathway architecture produces better matches to mouse ventral stream
2. CPC on a single pathway does not match mouse dorsal stream.
3. An architecture with parallel pathways trained with CPC can model both ventral and dorsal areas.
4. Supervised learning on parallel-pathways is not sufficient for dorsal match.
5. Functional specialization of the ventral and dorsal-like pathways in a CPC trained model.


They showed that CPC applied to an architecture that has two parallel pathways can model both the ventral and dorsal areas of mouse visual cortex. The downstream tasks of object recognition and motion discrimination also support the ventral-like and dorsal-like representations of the two pathways in the model.