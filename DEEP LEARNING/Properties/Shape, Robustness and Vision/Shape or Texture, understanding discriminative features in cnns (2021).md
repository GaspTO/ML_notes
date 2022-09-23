 Paper: [***Shape or Texture: Understanding Discriminative Features in CNNs***](https://arxiv.org/pdf/2101.11604.pdf)


# Overall
They uses [Esser's work](https://arxiv.org/pdf/2004.13166.pdf) to estimate how many neurons shape and texture need to be encoded and use this information to understand shape and texture in networks.

They also take feature vectors and train a read-out module which is a linear layer to try to perform some tasks that need shape information (similar to [[Origins Texture Bias in Convolutional Neural Networks (2019)]]).

They show that a network learns the majority of overall shape information at the first few epochs of training and that this information is largely encoded in the last few layers of a CNN.

This work puts into question some of the findings of [[Origins Texture Bias in Convolutional Neural Networks (2019)]].

Summary of both techniques:
![[Pasted image 20220728220513.png]]


# Methods
I will write this in more detail later. For now, consult the paper and [Esser's work](A Disentangling Invertible Interpretation Network for Explaining Latent Representations) 


# Results 
## Shape-Texture Dimensionality 
### Same stage, different nets and training data
![[Pasted image 20220728224129.png]]
(Stage-5 is the last one for resnet-50)

### Using another Methods to Compare
![[Pasted image 20220728224256.png|400]]

### Different parts of the network
![[Pasted image 20220728224421.png|400]]

This seems to contradict what the [[Origins Texture Bias in Convolutional Neural Networks (2019)]] paper says. In that paper, shape is constant accross stages and the texture increases, and here is the other way around - texture seems to be much more prevalent early on.

### Different parts of training
![[Pasted image 20220728224454.png]]


## Segmentation
Two types of segmentations: binary and semantic.
The hypothesis is that to segment you need very good shape knowledge (which is a bit debatable at first).

### Training settings
![[Pasted image 20220728232908.png]]
‘None’: a SEN with random weight initialization and without any training, ‘End-to-End’: the SEN and readout module trained in an end-to-end manner on either the binary (Bin) or semantic (Sem) segmentation ground truth. The None and End-to-End networks represent lower and upper bounds for encoding shape, respectively.


### Net stages
![[Pasted image 20220728232926.png]]
A surprising amount of shape information (i.e., Bin) can be extracted from stages f1, f2 and f3; however, these features lack high-level semantics to correlate with this shape information, which can be observed as the corresponding Sem performance is much lower. Figure 4 reveals this phenomenon; the horse and person are outlined even for the early stage binary masks, but are only labelled with correct per-pixel categorical assignments in the later stages.

###  Role of shape and texture neurons in segmentation
![[Pasted image 20220728235527.png]]

**Results**. Figure 7 illustrates the binary and semantic segmentation results in terms of mIoU obtained from training read-out modules on IN (left two) and SIN ((right two)) trained ResNet50s, respectively. It is clear that for the model biased towards texture (IN pretrained), keeping texture neurons while removing all other neurons results in a better performance than keeping only the shape neurons. In contrast, Fig. 7 (right two) shows that for shape-biased model (SIN pretrained), keeping shape-specific neurons achieves better performance than keeping only texture-specific neurons. These results support the hypothesis that the network trained on Stylized ImageNet (i.e., a shape biased model) is not only biased towards making predictions based on object shape, but more reliant on shape-specific neurons than a network trained on IN.