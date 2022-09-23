Paper: [***Does Enhanced Shape Bias Improve Neural Network Robustness To Common Corruptions***](https://arxiv.org/pdf/2104.09789v1.pdf)


# Overview
Shape bias gets maximized when combining edge information with stylization without including any texture information (Stylized Edges). However, for maximal corruption robustness, superimposing the image (and thus its textures) on these stylized edges is required. This, however, strongly reduces shape bias. In summary, corruption robustness seems to benefit most from style variation in the vicinity of the image manifold, while shape bias is mostly unrelated. Thus, image stylization is best interpreted as a strong data augmentation technique that encourages robust representations, regardless whether these representations are shape-based or not.

![[Pasted image 20220808043648.png]]


![[Pasted image 20220808043708.png]]



intra-stylization is nearly as effective as stylization based on paintings, implying that style need not necessarily be out-of-distribution for being useful.