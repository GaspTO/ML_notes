![[Pasted image 20220416230824.png]]


Their U-net trained with self-supervised was better than the supervised VGG-16 when compared to the brain using RSA:
![[Pasted image 20220416231125.png]]


Focusing on our model, we report results for every layer in Figure 8, analyzing the differences between encoder (or contraction path) and decoder (or expansion path) parts:
![[Pasted image 20220416235043.png]]

The decoder was closer to the early visual cortex than the encoder.

The hypothesis is that predictive coding theories suggest that the brain is trying to reconstruct the image under occlusion, using information in cortical feedback to probabilistically infer the content of missing image sections. Partial or complete occlusion, of objects for example, is commonplace in our visual environments.