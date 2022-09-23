Paper: [***Revisiting Self-Supervised Visual Representation Learning***](https://arxiv.org/pdf/1901.09005.pdf)


# Overview
They study self-supervised learning and get to a bunch of conclusions. The most important are:

1. Standard architecture design recipes do not necessarily translate from the fully-supervised to the self-supervised setting. Architecture choices which negligibly affect performance in the fully labeled setting, may significantly affect performance in the selfsupervised setting.
2. In contrast to previous observations with the AlexNet architecture, the quality of learned representations in CNN architectures with skip-connections does not degrade towards the end of the model.
3. Increasing the number of filters in a CNN model and, consequently, the size of the representation significantly and consistently increases the quality of the learned visual representations.
4. The evaluation procedure, where a linear model is trained on a fixed visual representation using stochastic gradient descent, is sensitive to the learning rate schedule and may take many epochs to converge.