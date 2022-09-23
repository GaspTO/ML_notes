**TODO: [11]**

There seems to be some concepts that are somehow connected. 

CNNs are far more different from the brain than initially thought. There are two major differences. The first is that it seems to make decisions (classify) by recognizing texture instead of understanding the shape of objects, as opposed to humans. The other is that it is vulnerable to adversarial attacks.


# ImageNet Accuracy
* It seems that the best architectures have higher shape bias.

* But, within the same architecture, when trying data augmentations, the higher the shape-bias the lower the imagenet classification.

* When it comes to self-supervised, it's not very straightforward as the paper says, but there is a slight tendency for the higher shape bias - higher accuracy. However, the theory is that data augmentation plays the role, because a supervised baseline trained with SimCLR augmentations scores even better than SimCLR, which also achieves the highest shape bias.

* Brendel & Bethge (2019) show that ImageNet can be largely be solved using only local information, indicating that a texture bias is an ecologically rational heuristic when solving this datase

* Better accuracy than training on imagenet is achieved if trained on stylized imagenet and normal imagenet, followed by fine-tuning on normal imagenet. Training on stylized imagenet and normal imagenet without the fine-tune yields worse results than training on normal imagenet alone.  

* It's easier for a network trained on the stylized to do well in imagenet than the other way around: ![[Pasted image 20220808172535.png|400]]


# Error Consistency
CNNs have higher error consistency between themselves than humans do, even in experiments with out-of-distribution datasets, where humans have higher accuracy [10].

# Architectures
## CNNs
Shape bias and Shape match seem to be influenced by architecture, but not texture match.
![[Pasted image 20220617032443.png|400]]

### Convolutions
In [1], they replace convolutions for local attention modules and the bias does not seem to change a lot.

## VOneResnet 
The VOneResnet was created by appending a fixed (non-learnable) layer in front of a resnet based on the Garbor functions that are common to the V1 part of the brain. Their results were to create this network that was more robust to adversarial attacks. 
On the brainscore platform, VoneResnet-50 has a better score regarding the V1 are than simple Resnet-50.

**Question:**
Do VoneResnet-50 classify more using shape than simple Resnet-50?



## Vision Transformers
Some works seem to support the idea that vision transformers are much similar to the brain than CNNs. Both in supporting their decision making using more shape information, but also by being more robust to adversarial attacks.

[7,8]



Recent work, however, seems to hypothise that it might be the training vision transformers go through to be able to work, which is far more data centered (larger and varied datasets).  These go as far as implying that if CNNs went through the same data regime, they would be as successful when it comes to achieving similairity to the brain.

[5]

At the same time, in the brainscore platform, it seems that vision transformers are not that similar to the brain. The best one is the [FrankRobWobv0](https://www.brain-score.org/model/968) 

Regarding shape bias, the following work shows VITs to be more shape biased [6] 
![[Pasted image 20220608130830.png|300]]

 
## Last Fully Connected Layers
in [1] *"shape accuracy decreased through the fully-connected layers of AlexNet’s classifier, and from ResNet pre-pool to post-pool, suggesting that these models’ classification layers remove shape information"*

[3] actually gets disappointed cause they don't see any relevant improvement on shape bias by self-supervised models, except simclr, which they conclude it's not even because of the self-supervised, but because the data augmentations.

My theory is that learning pro-shape representations before the fully connected layers is equally easy in both supervised and self-supervised setting, but both FC layer destroys this in both, so it doesn't matter whether we use self-sup or sup. It has only to do with that last layer.

# Robustness to OOD
* [3,4] imply that training a network to be more robust to adversarial training increases, as a consequence, the shape bias.

* According to [9], the reverse implication, that training on shape bias increases robustness, is also true. Training a ResNet on Stylized ImageNet improves and in some cases even surpasses humans:
![[Pasted image 20220617163940.png|400]]

* However, there is also evidence against the claim that shape bias increases shape robustness. In the table 4 of the appendix of [9] (A.9), the trained network that are the most robust to distortions is not the one only trained:
 ![[Pasted image 20220808172146.png|400]]
 
* [13] concludes that shape bias does not cause robustness, and that the stylized dataset could simply be seen as a data augmentation, just like many others that increase robustness:![[Pasted image 20220808181721.png|400]]

# Data
## Quantity
According to [1,3,5] the natural success of some models in shape bias and adversarial robustness seem to be a natural consequence of training on more data

## Data Augmentation
the effect of augmentations that reduce texture bias was additive in [1]

![[Pasted image 20220617044843.png|600]]

Interestingly, when using the Stylized-Imagenet in [9], training on SIN or SIN+IN does decrease top-1 accuracy, but when finetuned on IN, it actually increases the accuracy. This might be because the first IN+SIN creates better generalizations, but don't exploit the imagenet so well. But once you finetune it on IN, then it learns to exploit the IN texture, but only after using the good generalizations. 
![[Pasted image 20220617164459.png|500]]

This actually seems to be in line with with [1] when they say: *"shape accuracy decreased through the fully-connected layers of AlexNet’s classifier, and from ResNet pre-pool to post-pool, suggesting that these models’ classification layers remove shape information"*. There seems to be a core shape to create a generalization and the classification layers 


# Training Objective
* In [1,3] simclr seems to be good. But the data augmentation seems to be more important than objective.

# Training Time
[9]


# Interesting material
[It's the paper from the resnet-50-robust of brainscore which has the highest V1 score](Image Synthesis with a Single (Robust) Classifier)


# References
[1]  [The Origins and Prevalence of Texture Bias in Convolutional Neural Networks](https://arxiv.org/pdf/1911.09071.pdf)
[2]  [Simulating a Primary Visual Cortex at the Front of CNNs Improves Robustness to Image Perturbations](https://proceedings.neurips.cc/paper/2020/file/98b17f068d5d9b7668e19fb8ae470841-Paper.pdf)
[3]  [Partial success in closing the gap between human and machine vision](https://openreview.net/pdf?id=QkljT4mrfs)
[4]  [Interpreting Adversarially Trained Convolutional Neural Networks](https://arxiv.org/pdf/1905.09797.pdf)
[5]  [Are Transformers More Robust Than CNNs?](https://www.cs.jhu.edu/~alanlab/Pubs21/bai2021transformers.pdf)
[6]  [Are Convolutional Neural Networks or Transformers more like human vision?](https://arxiv.org/pdf/2105.07197.pdf)  
[7]  [Understanding Robustness of Transformers for Image Classification](https://openaccess.thecvf.com/content/ICCV2021/papers/Bhojanapalli_Understanding_Robustness_of_Transformers_for_Image_Classification_ICCV_2021_paper.pdf)
[8]  [On the Adversarial Robustness of Vision Transformers](https://arxiv.org/pdf/2103.15670.pdf)
[9]  [ImageNet-trained CNNs are biased towards texture; increasing shape bias improves accuracy and robustness](https://arxiv.org/pdf/1811.12231.pdf)
[10]  [Beyond accuracy: quantifying trial-by-trial behaviour of CNNs and humans by measuring error consistency](https://arxiv.org/abs/2006.16736)
[11]  [Shape or Texture: Understanding Discriminative Features in CNNs](https://arxiv.org/pdf/2101.11604.pdf)
[12]  [What shapes feature representations? Exploring datasets, architectures, and training](https://proceedings.neurips.cc/paper/2020/file/71e9c6620d381d60196ebe694840aaaa-Paper.pdf)
[13]  [Does Enhanced Shape Bias Improve Neural Network Robustness To Common Corruptions?](https://arxiv.org/pdf/2104.09789v1.pdf)


