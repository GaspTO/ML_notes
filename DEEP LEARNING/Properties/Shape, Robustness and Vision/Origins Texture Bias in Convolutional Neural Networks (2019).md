Paper:[***The Origins and Prevalence of Texture Bias in Convolutional Neural Networks***](https://arxiv.org/pdf/1911.09071.pdf)

This paper can be seen as a successor of [***ImageNet-trained CNNs are biased towards texture; increasing shape bias improves accuracy and robustness***](https://arxiv.org/pdf/1811.12231.pdf)

# Overall
We know the human brain classifies objects better according to shape more than according to texture. 

This paper focuses on one such result, namely that CNNs appear to make classifications based on superficial textural features rather than on the shape information preferentially used by humans.

From a computer vision prespective, texture bias might be related to adversarial attacks. It might also make it difficult to learn vision tasks that are important to human beings.

**This paper explores the origins of texture bias in ImageNet-trained CNNs, looking at the effects of data augmentation, training procedure, model architecture, and task**

## Summary of findings
**So what makes ImageNet-trained CNNs classify images by texture when humans do not? While we find effects of all of the factors investigated, the most important factor is the data itself. Our contributions are as follows:**

* naturalistic data augmentation involving color distortion, noise, and blur substantially decreases texture bias, whereas random-crop augmentation increases texture bias.
* We investigate the texture bias of networks trained with different self-supervised learning objectives.
* We show that architectures that perform better on ImageNet generally exhibit lower texture bias, but neither architectures designed to match the human visual system nor models that replace convolution with self-attention have texture biases substantially different from ordinary CNNs.
* We separate the extent to which shape information is represented in an ImageNet-trained model from how much it contributes to the model’s classification decisions. We find that it is possible to extract more shape information from a CNN’s later layers than is reflected in the model’s classifications, and study how this information loss occurs as data flows through a network.

# Dataset
These datasets will be very important for the whole paper.

* **Geirhos Style-Transfer (GST) dataset** contains images generated using neural style transfer, which combines the content (shape) of one target natural image with the style (texture) of another. The dataset consists of 1200 images rendered from 16 shape classes, with 10 exemplars each, and 16 texture classes, with 3 exemplars each.
* **Navon dataset**. Navon figures consist of a large letter (“shape”) rendered in small copies of another letter (“texture”). Unlike the GST stimuli, the primitives for shape and texture are identical apart from scale, allowing for a more direct comparison of the two feature types.  
* **ImageNet-C dataset** consists of ImageNet images corrupted by different kinds of noise (e.g. shot noise, fog, motion blur). Here, we take noise type as “texture” and ImageNet class as “shape”. The original dataset contains 19 textures each at 5 levels, with 1000 ImageNet classes per level and 50 exemplars per class. To balance shape and texture, for each of 5 subsets of the data (dataset “versions”), we subsampled 19 shapes, yielding a total of 90,250 items per version.


# Metrics
**Shape bias** is percentage of the time it classified images from the GST dataset according to shape, provided it classified either shape or texture correctly. 

A model shape-biased if its shape bias is > 50%, and texture-biased if it is < 50%. 

Throughout, **shape match** and **texture match** indicate the percentage of the time the model chose the image’s correct shape or texture label, respectively, as opposed to choosing some other label.


# CNNs can learn shape as easily as texture
CNNs tend to be texture driven. 
Is this because of CNN architecture bias? Training process? Data?

For each of the Geirhos-Style Transfer, Navon, and ImageNet-C datasets, we trained AlexNet and ResNet-50 models to classify images according to (a) shape, or (b) texture, using 5%, 10%, 20%, 30%, ..., 90%, 100% of the training data

![[Pasted image 20220513125026.png|500]]

nothing prevents AlexNet and ResNet-50 from learning to classify ambiguous images by their shape. We should be cautious in comparing shape and texture performance on GST and ImageNet-C given the unknown in-principle recoverability of shape and texture from these dataset


# The role of data augmentation in texture bias
#### **Random-crop data augmentation increases texture bias.**
We hypothesized that random-crop augmentation might remove global shape information from the image, since for large central objects, randomly varying parts of the object’s shape may appear in the crop, rendering shape a less reliable feature relative to texture. 

Center-crop augmentation reduced texture bias relative to random-crop augmentation; and center-crop models had higher shape bias than random-crop models throughout the training process. Further, texture bias decreased as minimum crop area used in random-crop augmentation increased.

![[Pasted image 20220513134829.png]]

#### **Appearance-modifying data augmentation reduces texture bias.** 
The majority of photographs in major computer vision datasets are taken in well-lit environments. We hypothesized that the development of human-like shape representations might require more diverse training data than what is present in ImageNet.
we tested whether a similar effect could be achieved with more naturalistic forms of data augmentation that do not require training on stylized images, and that more closely resemble the variation encountered by humans.

![[Pasted image 20220513140541.png]]

# Effect of training objective
To test this hypothesis, we compared the shape bias of models trained with standard supervised learning to models trained with self-supervised objectives different from supervised classification.

Each self-supervised objective with up to two architecture backbones: AlexNet and ResNet-50 v2.

Self-Supervised Objectives:
* Rotation classification.
* Exemplar.
* BigBiGAN.
* SimCLR.

**Evaluation of shape bias**. To facilitate comparison across supervised and self-supervised tasks, we trained classifiers on top of the learned representations using the standard ImageNet dataset and softmax cross-entropy objective, freezing all convolution layers and reinitializing and retraining later layers

**Results**. We found effects of both objective and base architecture on texture bias (Table 4).

![[Pasted image 20220513143558.png]]


* The rotation model had significantly lower texture bias than supervised models for both architectures; this may be because rotationally-invariant texture features are not useful for performing the rotation classification task. In general, shape match was higher for models with AlexNet than ResNet-50 architecture

* They say that *"The effects of architecture and task appear to be largely independent: the rank order of shape bias across tasks was similar for the two model architectures."*, but they only test 2 techniques for AlexNet and 4 with Resnet and the rank order isn't even the same....

* Amongst the unsupervised ResNet-50 models, SimCLR had the lowest texture bias. However, a supervised baseline with the same augmentation had a still-lower texture bias, suggesting that **augmentation rather than objective** was the more important factor.


## Effect of architecture
Figure 4 shows the shape bias, shape match, and texture match of 16 high-performing ImageNet models trained with the same hyperparameters. 

![[Pasted image 20220513144635.png]]

Both shape bias and shape match correlated with ImageNet top-1 accuracy, whereas texture match had no significant relationship.

These results suggest that models selected for high ImageNet. These results suggest that models selected for high ImageNet Top-1 are more effective at extracting shape information. However, the asymmetry with Figure 3 shows that models selected for high shape bias do not necessarily have high ImageNet top-1. However, the asymmetry with Figure 3 shows that models selected for high shape bias do not necessarily have high ImageNet top-1.

![[Pasted image 20220513144922.png|200]]

**In other words: Success in imagenet implies shape bias, but shape bias does not imply imagenet success**


## **Shape bias of attention vs. convolution.**
*Ramachandran et al*. recently proposed a novel approach to image classification that replaces every convolutional layer in ResNet-50 v1 with local attention, where attention weights are determined based on both relative spatial position and content. 

The attention model had a shape bias of 20.2% (shape match: 12.8%; texture match: 50.7%), similar to the baseline’s shape bias of 23.2% (shape match: 14.4%; texture match: 47.7%). Thus, **use of attention in place of convolution appears to have little effect upon texture bias**

# Degree of representation of shape and texture in ImageNet models
**(See  [Shape or Texture: Understanding Discriminative Features in CNNs](https://arxiv.org/pdf/2101.11604.pdf) which has some evidence to contradict the following)**

Standard ImageNet-trained CNNs are biased towards texture in their classification decisions, but this does not rule out the possibility that shape information is still represented in layers of the model prior to the output.

We trained linear multinomial logistic regression classifiers on two classification tasks. Taking as input activations from a layer of a frozen, ImageNet-trained model, the classifier predicted either (i) the shape of a GST image or (ii) its texture

**Results**. We found that, despite the high texture bias of AlexNet and ResNet-50, shape could nonetheless be decoded with high accuracy (Figures 5 and C.4). In fact, it was possible to classify shape (77.9%) more accurately than texture (65.6%) throughout the convolutional layers of AlexNet. In ResNet-50, while decoding accuracy for texture (80.9%) was higher than for shape (66.2%) for pre-pool, shape accuracy was still high. Interestingly, shape accuracy decreased through the fully-connected layers of AlexNet’s classifier, and from ResNet pre-pool to post-pool, suggesting that these models’ classification layers remove shape information.


![[Pasted image 20220513154218.png|300]]

![[Pasted image 20220717010646.png]]