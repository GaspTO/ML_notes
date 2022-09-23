One of the hypothesis (that I've been the most interested in recently) is the idea
that shape understandment in networks exists, but is erradicated by some quick fit on texture that might have to do with the last feedforward layer.

There is however an interesting question:

**Can this feedforward layer classify shape if necessary?**

The way you can measure this is by asking the network to classify on datasets that don't have texture, or have a non-information texture like *sketch*, *stylized*, *edge*, *silhouette*, and *texture-shape cue conflict*, respectively:

![[Pasted image 20220726194251.png]]



We will refer these 5 datases as the "*datasets*". 
* Silhouette has only outside shape
* Sketch has shape outside but also inside (like furr, eyes...), and no texture
* Edge has shape outside, wrong shapes inside, and no texture
* Stylized has right shapes outside and some inside, and wrong texture (Although it's a very weird dataset)
* Cue-Conflict has one shape, another texture.

We're now going to look at the correlations given the results of brainscore
## Imagenet
### Sketch
![[Pasted image 20220726194522.png|400]]

### Stylized
![[Pasted image 20220726194628.png|400]]

### Edge
![[Pasted image 20220726194654.png|400]]

### Silhoutte
![[Pasted image 20220726194839.png|400]]

### texture-shape cue conflict
the accuracy of this dataset is the shape
![[Pasted image 20220726194921.png|400]]

### Discussion
**Sketch**, **Stylized** and **Sillhoute** have very well defined correlations with imagenet accuracy. There seems to be some correlation with **Edge**, and not much with **Cue-Conflict**.

The **Sketch** is particularly interesting because it gets almost 100% accuracy in some models, and, yet, it only has shape. It seems that the models can decode shape even with the multi-layer perceptron, but when they have the right texture, they give it priority.

## Cue-conflict
### Sketch
![[Pasted image 20220726213404.png|400]]

### Stylized
![[Pasted image 20220726213434.png|400]]
### Edge
![[Pasted image 20220727001815.png|400]]
### Silhouette
![[Pasted image 20220727001355.png|400]]

### Discussion
The seems to be some correlation between the shape accuracy, the tendency to classify according to shape and not texture, and the ability to classify the other datasets, but it's not too much.
It's also worth noticing that it's harder to classify the shape in the cue-conflict than in the other datasets. For example, the cue-conflict barely passes 0.3 and the others pass this one more easily.


## Edge
## Sketch
![[Pasted image 20220727002101.png|400]]

## Stylized
![[Pasted image 20220727002154.png|400]]

## Silhouette
![[Pasted image 20220727002323.png|400]]

## Discussion
It seems like edge accuracy correlates well with all the other datasets, except cue-conflict, which seems to correlate only a bit.

### Sketch
![[Pasted image 20220727010408.png|400]]

###  Edge
![[Pasted image 20220727010428.png|400]]

### Silhouette
![[Pasted image 20220727010536.png|400]]


## Stylized
![[Pasted image 20220727010821.png|400]]


# Discussion
The hypotesis seems to be the following: networks are more biased to texture, and will use it when possible, but, in lack of it, they still know how to use shape to orient themselves.
That's why the shape match (accuracy on cue-conflict) doesn't correlate very well with anything. A model can know a lot of shape but be biased towards texture, leading to be good in all the 4 datasets except the cue-conflict. Or, it can be very good at shape and be biased to it as well, whcih would result also in good accuracy in all datasets, including cue-conflict.

But there seems to be a slight correlation between shape bias and accuracy on the other datasets, which might imply that either **(1)** being really good at shape tilts it slight towards shape bias, or **(2)** that shape bias models are better at shape. 

The **(2)** seems obviously true, but it isn't. In fact, it would be a very interesting thing to explore. Once we have the shape bias benchmark, we can compare shape bias vs the datasets and see if this holds.