This is a very short paper resulting from the Brain score workshop and it is currently the best model in the platform.

# Overview
The focus of this work has been on training an ANN performing well on the **V1** alignment benchmark by Freeman et al and the **behavioral** alignment benchmark by Rajalingham et al. Competitive results in the other evaluation categories V2, V4 and IT fell off with it.

Augmentation techniques used in the proposed training pipeline include geometric transformations, color space augmentations, random erasing and adding noise.


# Background

The positive correlation between adversarial robustness and the ANNs capability to explain brain V1 variance has been demonstrated in literature. To improve an ANNs behavior-similarity, a first intuitive approach would be to reduce the
data distribution gap between an ANNs training data and the behavior test data. This might be connected to the usage of texture as opposed to shape by CNNs, which as seen in this [[Origins Texture Bias in Convolutional Neural Networks (2019)|paper]], seems to be a property of data more than training process or architecture.

For many ImageNet-pretrained vision ANNs, the current best training practice includes **random cropping** and **resizing** at training time, while simply extracting a centered square (center cropping) at test time. This leads to different object sizes at train and test time, resulting in potentially wasted object recognition performance due to a distribution shift. The problem can be by increasing the resizing and cropping resolutions at test time.

**Weight averaging (WA**) is a well-known generalization technique in deep learning. In its simplest form, averaging the learned weights of two or more architectural identical ANNs increases the resulting ANN’s performance. It also leads to wider optima than those found by stochastic gradient descent during training.

# Methods and Results
The above-mentioned methods combine different concepts to improve an ANNs Brain-Score benchmarks. They represent a mix of regularization, generalization, data distribution shifts and adversarial robustness. Despite V1 alignment and adversarial robustness as well as behavioral alignment and classification accuracy generally don’t correlate very strong, the robustness and accuracy gains from some techniques do correlate with the corresponding V1 and behavior metrics.

An intuitive approach to train ANNs to be not distracted by the background image presented in the behavioral benchmark would be a adjusted form of the CutMix augmentation.

The augmentations used in the ablation study (referred to as "heavy augmentations") include image shifting, rotation, elastic deformation, brightness and contrast adjustments, motion blurring, cutout, adding Gaussian and ISO noise.

The adversarial robust training used for improving the V1 metric adversely effects the ANNs object classification performance, and thus the behavior score. To overcome this, the final ANN was first trained adversarial robust under the optimal parameters (see Table 2). Subsequently the first half of the ANNs layers were frozen (including Batch Norm statistics), and it was fine-tuned for behavior response using the proposed techniques in 1. The layers that explain most of V1 variance are likely to be found among the first half of all layers. The behavioral response however is read out in the penultimate layer. The presented ablation studies were conducted without any layer freezing. 

![[Pasted image 20220513172140.png|500]]

![[Pasted image 20220513172219.png|500]]


**Generalization.** The proposed methods were validated on a ResNet50 to demonstrate their generalizability. Similar improvements in explained V1 variance and behavioral response can be achieved compared to the studied EfficientNet-B1 architecture.

# Discussion
The combination of adversarial robust training, layer freezing and behavioral metric aware data distribution shifting leads to an improved explained V1 variance and a competitive behavioral metric response. Despite the proposed methods lead to a winning model in the Brain-Score competition 2022 and present a new state-of-the-art performance in the Brain-Score benchmark overall, it still remains unclear whether they lead towards biologically more plausible ANNs or if they are just ways to exploit the Brain-Score benchmark. However, the further development of brain-like ANNs should address the influence of an optimized training pipeline as it might be a way to push a biologically
plausible ANN towards its frontiers of explaining primates visual cortex responses.