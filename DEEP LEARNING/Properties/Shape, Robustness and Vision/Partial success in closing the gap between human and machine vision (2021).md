Paper: [***Partial success in closing the gap between human and machine vision***](https://openreview.net/pdf?id=QkljT4mrfs)

# Resources
[**Github**](https://github.com/bethgelab/model-vs-human)


# Overview 
They pretty much create 17 OOD (out of distribution) datasets to evaluate  and compare multiple models and humans.  They compare self-supervised
The conclusions they've reached is that models are closing the gap between humans and models. Also, the training data (size) seems to be more important than anything else. Those OOD datasets have in common that they are designed to test ImageNet-trained models.


# Methods
## Models
In order to disentangle the influence of objective function, architecture and training dataset size, we tested a total of 52 models: 24 standard ImageNet-trained CNNs, 8 selfsupervised models,2 6 Big Transfer models, 5 adversarially trained models, 5 vision transformers, two semi-weakly supervised models as well as Noisy Student and CLIP.

## Metrics
**Accuracy difference A(m)** is a simple aggregate measure that compares the accuracy of a machine m to the accuracy of human observers in different out-of-distribution tests (see paper for the formula).

**Observed consistency O(m)** measures the fraction of samples for which humans and a model m get the same sample either both right or both wrong see paper for the formula).

**Error consistency E(m)** Error consistency E(m) tracks whether there is above-chance consistency. This is an important distinction, since e.g. two decision makers with 95% accuracy each will have at least 90% observed consistency, even if their 5% errors occur on non-overlapping subsets of the test data (intuitively, they both get most images correct and thus observed overlap is high). To this end, error consistency (a.k.a. Cohen’s kappa) indicates whether the observed consistency is larger than what could have been expected given two independent binomial decision makers with matched accuracy


# Results
**Picture for reference:**
![[Pasted image 20220614024805.png]]
![[Pasted image 20220614024826.png]]
![[Pasted image 20220614024840.png]]
## Robustness across models
### <span style="color:DarkGoldenRod"> Self-supervised models </span>
* Figure 2 compares the generalisation behaviour of eight self-supervised models in orange (PIRL, MoCo, MoCoV2, InfoMin, InsDis, SimCLR-x1, SimCLR-x2, SimCLR-x4) with 24 standard supervised models (<span style="color:gray">grey</span>).
* We find only marginal differences between self-supervised and supervised models: Across distortion types, self-supervised networks are well within the range of their poorly generalising supervised counterparts.
* However, there is one exception: the three SimCLR variants show strong generalisation improvements on uniform noise, low contrast, and high-pass images, where they are the three top performing self-supervised networks — quite remarkable given that SimCLR models were trained on a different set of augmentations (random crop with flip and resize, colour distortion, and Gaussian blur).
* The augmentation scheme made more of a difference than the self-supervised objective. (Previously observed by [[Origins Texture Bias in Convolutional Neural Networks (2019)|paper]] which also noted the independence from architecture).

### <span style="color:steelblue"> Adversarially trained models </span>
* They use adversarial training.
* They train five models with a ResNet-50 architecture and different accuracy-robustness tradeoffs.
* They found that the stronger the model is trained adversarially (darker shades of blue in Figure 2), the more susceptible it becomes to (random) image degradations. Most strikingly, a simple rotation by 90 degrees leads to a 50% drop in classification accuracy. Adversarial robustness seems to come at the cost of increased vulnerability to large-scale perturbations.
* Also, they observe a perfect relationship between shape bias and the degree of adversarial training, a big step in the direction of human shape bias (and a stronger shape bias than nearly all other models).

### <span style="color:darkgreen"> Vision transformers </span>
* ViT-L exceeds humans in OOD accuracy.
* They seem to have great performance because of the enormous amount of data they are trained on.

### <span style="color:Purple"> BiT-M, SWSL and Noisy Student </span>
* The biggest effect on OOD robustness comes from training on larger datasets, not from advanced architectures.
* Even transformers with better architecture lose to those trained with bigger datasets

