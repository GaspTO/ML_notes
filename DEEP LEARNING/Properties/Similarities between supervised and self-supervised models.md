Paper: [***On the surprising similarities between supervised and self-supervised models***](https://arxiv.org/pdf/2010.08377.pdf)


# Overview
**Questions:**
1)  Are self-supervised models more robust towards distortions?
2) Do self-supervised models make similar errors as either humans or supervised models? 
3) Do self-supervised models recognise objects by texture or shape?

**Findings:**
Current self-supervised CNNs share four key characteristics of their supervised counterparts: 
1. similar, but relatively poor noise robustness (with the notable exception of SimCLR)
2. non-human category-level error patterns
3. non-human image-level error patterns (yet high similarity to supervised model errors) and
4. a bias towards texture

Their results suggest that the strategies learned through today’s supervised and self-supervised training objectives end up being surprisingly similar, but distant from human-like behaviour.


# Methods
**Self-supervised models:** IRL, MoCo, MoCoV2, InfoMin, InsDis, SimCLR-x1, SimCLR-x2, SimCLR-x4.

**Supervised models:** AlexNet, VGG, Squeezenet, DenseNet, Inception, ResNet, ShuffleNet, MobileNet, ResNeXt, WideResNet and MNASNet (all the available pre-trained pytorch models).

They compare supervised and self-supervised models against a comprehensive set of openly available human psychophysical data totalling over 130,000 trials [4, 5].
