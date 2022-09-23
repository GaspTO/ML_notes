- Paper:***An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale***

# Overview 
It's just a normal [[Transformer (to finish)|transformer]]. Transformers hadn't been successful before, mainly because the transformer doesn't have the image bias that CNNs do. To solve this, this paper pre-trains the model using insanely big datasets and then fine tunes them to the test dataset, like imagenet.

Another way to mitigate this problem is to use the feautre maps of some CNN.

Another important detail is that the self-attention is not done from every pixel to every pixel, which would be too complex. Instead, the images are broken into patches, small image pieces, and then fed to the transformer as if they are tokens. 

Positional encodings about the position of each patch is added.

![[ViT model overview.png|500]]

___
#Vision #Architecture  #Transformer #Attention 


