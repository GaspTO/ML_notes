Paper: [***Are Transformers More Robust Than CNNs?***](https://www.cs.jhu.edu/~alanlab/Pubs21/bai2021transformers.pdf)

# Overview
This paper challenges the current *finding* that Transformers appear to be much more robust (to adversarial attacks) than CNNs. 

## Definition
Conventional learning paradigm assumes training data and testing data are drawn from the same distribution. This assumption generally does not hold, especially in the real-world case where the underlying distribution is too complicated to be covered in a (limitedsized) dataset. To properly access model performance in the wild, a set of robustness generalization benchmarks have been built , e.g., ImageNet-C, Stylized-ImageNet, ImageNet-A... 

Another standard surrogate for testing model robustness is via adversarial attacks, where the attackers deliberately add small perturbations or patches to input images.

**In this work, both robustness generalization and adversarial robustness are considered in our robustness evaluation suite.**

## Problem
The issues raised by this paper are that the studies that conclude this compare CNNs and Transformers under different and unfair conditions. In specifc, they mention two:
1. Transformers and CNNs are not compared at the same model scale, e.g., a small CNN, ResNet50 (∼25 million parameters).
2. the training frameworks applied to Transformers and CNNs are distinct from each other (e.g., training datasets, number of epochs, and augmentation strategies are all different), while little efforts are devoted on ablating the corresponding effects.

## Methods
They focus on the comparisons between Small Data-efficient image Transformer (DeiT-S) and ResNet-50, as they have similar model capacity (22 and 25 million parameters, respectively) and achieve similar performance on ImageNet (76.8% and 76.9% top-1 accuracy).

 Their evaluation suite accesses model robustness in two ways: 
 1) adversarial robustness, where the attackers can actively and aggressively manipulate inputs to approximate the worst-case scenario.
 2) generalization on out-of-distribution samples, including common image corruptions (ImageNet-C), texture-shape cue conflicting stimuli (Stylized-ImageNet) and natural adversarial examples (ImageNet-A). 

## Conclusions
They find that Transformers are not more robust than CNNs— if CNNs are allowed to properly adopt Transformers’ training recipes, then these two types of models will attain similar robustness on defending against both perturbation-based adversarial attacks and patch-based adversarial attacks.

While for generalization on out-of-distribution samples, we find Transformers can still substantially outperform CNNs even without the needs of pre-training on sufficiently large (external) datasets.

Additionally, our ablations show that adopting Transformer’s self-attention-like architecture is the key for achieving strong robustness on these out-of-distribution samples, while tuning other training setups will only yield subtle effects here.


# Training
Read the article to know the details, but there are still some differences in this paper in the way they train both networks. The similarities are the number of epochs, dataset (not data augmentation, their transformer uses some basic data augmentation) and the number of parameters. The classification error achieved is similar.


# Results on Adversarial Robustness
We consider both perturbation-based attacks (i.e., PGD and AutoAttack) and patch-based attacks (i.e., TPA) for robustness evaluations.


## Perturbation-Based Attacks
They are both bad with a high perturbation radius (4/255), but when it is smaller (0.001) the transformer is better, even though both were very bad.
![[Pasted image 20220524233228.png]]

### Adversarial Training
![[Pasted image 20220524233445.png]]

## Robustness to Patch-Based Attacks
![[Pasted image 20220524233701.png]]

Interestingly, as shown in Table 3, though both models attain similar clean image accuracy, DeiT-S substantially outperforms ResNet-50 by 28% on defending against TPA. We conjecture such huge performance gap is originated from the differences in training setups; more specifically, it may be resulted by the fact DeiT-S by default use strong data augmentation strategies while ResNet-50 use none of them.

The augmentation strategies like CutMix already naïvely introduce occlusion or image/patch mixing during training, therefore are potentially helpful for securing model robustness against patch-based adversarial attacks

**To verify the hypothesis above, we next ablate how strong augmentation strategies in DeiT-S (i.e., RandAug, Mixup and CutMix) affect ResNet-50’s robustness**

![[Pasted image 20220524233853.png]]


# Robustness on Out-of-distribution Samples
In addition to adversarial robustness, we are also interested in comparing the robustness of CNNs and Transformers on out-of-distribution samples. We hereby select three datasets, i.e., **ImageNet-A**, **ImageNet-C** and **Stylized ImageNet**, to capture the different aspects of out-of-distribution robustness.

**A simple baseline here is that we completely adopt the recipes of DeiT-S to train ResNet-50, denoted as ResNet-50*.** Specifically, this ResNet-50* will be trained with AdamW optimizer, cosine learning rate scheduler and strong data augmentation strategies. Nonetheless, as reported in Table 5, ResNet-50* only marginally improves ResNet-50 on ImageNet-A (+1.3%) and ImageNet-C (+2.3), which is still much worse than DeiT-S on robustness generalization.


![[Pasted image 20220524235200.png]]

It is possible that completely adopting the recipes of DeiT-S overly regularizes the training of ResNet50, leading to suboptimal performance. To this end, we next seek to discover the “best” setups to train ResNet-50, by ablating learning rate scheduler (step decay vs. cosine decay), optimizer (M-SGD vs. AdamW) and augmentation strategies (RandAug, Mixup and CutMix) progressively

![[Pasted image 20220524235550.png]]

We hereby examine whether these augmentation strategies affect robustness generalization. The performance of ResNet-50 trained with different combinations of augmentation strategies is reported in Table 7

![[Pasted image 20220524235618.png]]

Nonetheless, interestingly, we note DeiT-S still shows much stronger robustness generalization on out-of-distribution samples than our “best” ResNet-50.