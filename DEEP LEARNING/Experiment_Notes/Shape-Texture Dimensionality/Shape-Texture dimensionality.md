# Overview
After implementing the shape match and shape bias benchmarks for brainscore, I have been involved with the concept dimensionality idea ( [**_Shape or Texture: Understanding Discriminative Features in CNNs_**](https://arxiv.org/pdf/2101.11604.pdf))

I have implemented it (https://github.com/GaspTO/ML). I have noticed that there is a huge discrepancy between paper and implementation.  Their original implementation is very loose. I have implemented 4 versions, "dim4" - the most loose, equal to their implementation, to "dim2", a more strict one. There are two new ones "dim1" and "dim0" that, instead of summing the $s_i$ scores, it sums their absolute value. The difference between the two is that "dim1" does a softmax on them, just like the original versions, where "dim0" simply normalizes them proportionally.

The goal is to connect the two metrics: shape bias and shape dim, and utilize the two datasets: stylizedVOC and cue-conflict

# General Notes
*  Right now, 22 September 2022, we are only studying StylizedVOC. This dataset is very clean. For every shape there are 5 instances with 5 different textures. The Cue-conflict dataset isn't as uniform. I will first study the StylizedVOC, and only then go to the cue-conflict.

# Questions / Study plan
* logits layer
* all neurons
* Make layer by layer study. In the first experiment, there is a positive correlation between score_shapes and shape_match, which makes sense since experiment 1 is before the softmax. 
* Study for cue-conflict
* Study relationship between shape dimensionality and the layers of V1, V2, V4, and IT.
* Should I do, medians in scores, instead of averages? Maybe see a couple network distributions
* If a network's features are always constant (collapsed), then the neurons are going to still show correlations when passing from an input to another input with, say, different texture. So, if some neurons are constant in some scenarios, and only activate in specific cases, they will appear to "explain", say, shape, when in reality they are simply not reacting. -> Motivation (This is what justifies testing it with the shape bias metric). ---- Ok, this actually has a problem. If they all have the same value, then the variance will be 0 and the score will have a 0 denominator. **Maybe we should test a network that outputs random features and see what hapens.**


# Pitfalls
* Careful to reproduce the analysis because the StylizedVOC has a randomized component.