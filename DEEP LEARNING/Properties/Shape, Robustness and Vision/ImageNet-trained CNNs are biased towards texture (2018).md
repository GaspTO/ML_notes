Paper: [***ImageNet-trained CNNs are biased towards texture; increasing shape bias improves accuracy and robustness***](https://arxiv.org/pdf/1811.12231.pdf)

# Overview
This paper finds out that CNNs trained in the ImageNet are biased towards the texture of image and not their shape. **This is the first paper** that starts the problem of **shape vs texture**.  

They test human beings are notice that we tend to use more shape than texture, contradicting neural networks.

In addition, they seem to figure out that this bias comes from the imagenet and not the networks, something following papers seem to confirm.


# Methods
## Datasets
They run an object classification experiment on human beings using 6 tasks:

**Original**. 160 natural colour images of objects (10 per category) with white background.

**Greyscale**. Images from Original data set converted to greyscale using skimage.color.rgb2gray. For CNNs, greyscale images were stacked along the colour channel.

**Silhouette Images**. from Original data set converted to silhouette images showing an entirely black object on a white background.

**Edges Images**. from Original data set converted to an edge-based representation.

**Texture**. 48 natural colour images of textures (3 per category). Typically the textures consist of full-width patches of an animal (e.g. skin or fur) or, in particular for man-made objects, of images with many repetitions of the same objects (e.g. many bottles next to each other).

It is important to note that we only selected object and texture images that were correctly classified by all four networks.

**Cue conflict**. Images generated using iterative style transfer between an image of the Texture data set (as style) and an image from the Original data set (as content). We generated a total of 1280 cue conflict images (80 per category), which allows for presentation to human observers within a single experimental session. An example of a cue conflict image:
![[Pasted image 20220527014248.png|500]]


##  Stylized-ImageNet (SIN)
Starting from ImageNet we constructed a new data set (termed Stylized-ImageNet or SIN) by stripping every single image of its original texture and replacing it with the style of a randomly selected painting.

Here are a few examples of how different stylizations of the same ImageNet image can look like:![[Pasted image 20220527013830.png|600]]




# Results
## Humans vs CNNs
* Almost all object and texture images (Original and Texture data set) were recognised correctly by both CNNs and humans.
* Greyscale versions of the objects, which still contain both shape and texture, were recognised equally well.
* When object outlines were filled in with black colour to generate a silhouette, CNN recognition accuracies were much lower than human accuracies.
* This was even more pronounced for edge stimuli, indicating that human observers cope much better with images that have little to no texture information.
![[Pasted image 20220527014401.png]]

* The results on the cue conflicted images can be seen in the next image. Human observers show a striking bias towards responding with the shape category, as opposed to CNNs, which show a clear bias towards responding with the texture category.
![[Pasted image 20220527014912.png]]

## Overcoming the Texture Bias in CNNs
One reason for the texture bias might be the training task itself: from Brendel & Bethge (2019) we know that ImageNet can be solved to high accuracy using only local information.

In order to test this hypothesis we train a ResNet-50 on our *Stylized-ImageNet (SIN)* data set in which we replaced the object-related local texture information with the uninformative style of randomly selected artistic paintings.

A standard ResNet-50 trained and evaluated on *Stylized-ImageNet (SIN)* achieves 79.0% top-5 accuracy, while 92.9% on ImageNet. This performance difference indicates that SIN is a much harder task than IN since textures are no longer predictive. **ImageNet features generalise poorly to SIN (only 16.4% top-5 accuracy); yet features learned on SIN generalise very well to ImageNet (82.6% top-5 accuracy without any fine-tuning).**


The SIN-trained ResNet-50 shows a much stronger shape bias in our cue conflict experiment (Figure 5), which increases from 22% for a IN-trained model to 81%. In many categories the shape bias is almost as strong as for humans.
![[Pasted image 20220527021133.png]]


## Robustness and Accuracy of Shape-Based Representations
In addition to the IN- and SIN-trained ResNet-50 architecture they additionally analyse two joint training schemes:
• Training jointly on SIN and IN.
• Training jointly on SIN and IN with fine-tuning on IN. They refer to this model as *Shape-ResNet*.


We then compared these models with a vanilla ResNet-50 on three experiments:
1. classification performance on IN
2. transfer to Pascal VOC 2007
3. robustness against image perturbations.

![[Pasted image 20220527023349.png]]

**Classification**. performance Shape-ResNet surpasses the vanilla ResNet in terms of top-1 and top-5 ImageNet validation accuracy as reported in Table 2. This indicates that SIN may be a useful data augmentation on ImageNet that can improve model performance without any architectural changes.

**Transfer learning**. We tested the representations of each model as backbone features for Faster R-CNN on Pascal VOC 2007. Incorporating SIN in the training data substantially improves object detection performance from 70.7 to 75.1 mAP50 as shown in Table 2. This is in line with the intuition that for object detection, a shape-based representation is more beneficial than a texture-based representation, since the ground truth rectangles encompassing an object are by design aligned with global object shape.

**Robustness against distortions**. We systematically tested how model accuracies degrade if images are distorted by uniform or phase noise, contrast changes, high- and low-pass filtering or eidolon perturbations.  The results of this comparison, including human data for reference, are visualised in Figure 6. While lacking a few percent accuracy on undistorted images, the SIN-trained network outperforms the IN-trained CNN on almost all image manipulations. (Low-pass filtering / blurring is the only distortion type on which SIN-trained networks are more susceptible, which might be due to the over-representation of high frequency signals in SIN through paintings and the reliance on sharp edges.) The SIN-trained ResNet-50 approaches human-level distortion robustness—despite never seeing any of the distortions during training

![[Pasted image 20220527030143.png|500]]