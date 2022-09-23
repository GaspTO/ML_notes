Paper: [***U-Net: Convolutional Networks for Biomedical Image Segmentation***](https://arxiv.org/pdf/1505.04597.pdf)

# Overview
U-Net is an architecture that hits very good results and has the shape of a U:
![[Pasted image 20220114185609.png]]
The key guidelines are:
	-Double the number of feature maps when shrinking using a convolution.
	-There are skip connections that concatenate from one side to another.
	-Use cross-entropy
	-They use data augmentation: shift, rotational, robustness to deformations and gray value variations. **IMPORTANT:** Especially random elastic deformations of the training samples seem to be the key concept to train a segmentation network with **very few** annotated images.
	
	
	
	




___
#Vision #SemanticSegmentation #MedicalSegmentation #Architecture 