

Papers:
*  [What shapes feature representations? Exploring datasets, architectures, and training](https://proceedings.neurips.cc/paper/2020/file/71e9c6620d381d60196ebe694840aaaa-Paper.pdf)

This paper is about decoding shape, texture and color. They use two datasets, Navon (texture, shape) and Trifeatures (texture, shape, color).
![[Pasted image 20220805035529.png|500]]

## Train on one feature

First, they train an Alexnet on Trifeature and Navon on one feature at a time and try to retrieve each feature. Obviously, the feature trained for is always the most easily decodable, and the others were supressed:
![[Pasted image 20220805035753.png|500]]
![[Pasted image 20220805035808.png|500]]

## What if two features redundantly predict the label?
They train on correlated features with a probability of 1. For example, if they are training on shape and color, then, for example, every time you see a circle, it is of color red. In other words, there is a label per pair (shape, color). 

The question now is which feature, color or shape, is the model going to use?

![[Pasted image 20220805040431.png|300]]

## What if one feature perfectly predicts the label, but another only partially predicts it?
![[Pasted image 20220805042150.png|500]]

The target feature was always enhanced (higher decodability from the trained than the untrained model), whereas the correlated non-target feature was generally preserved (no change) or suppressed (Figure 5). Surprisingly, the level of suppression was more or less constant across degree of correlation, except at high correlations.

## Do models prefer reliable but difficult features, or easy but less reliable ones?
![[Pasted image 20220805042319.png]]

If the easier is not predictive at all, then it might choose the harder, but if the easier is predictive enough, it will choose it.


## What affects the representational similarity between models?
![[Pasted image 20220805042446.png|500]]