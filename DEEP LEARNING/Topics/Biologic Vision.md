# Anatonomy
![[Pasted image 20220329004446.png|250]]

## Ventral and Dorsal Streams
In the **two-streams hypothesis**, the ventral stream is the *what* pathway, that leads the visual information from the retina to the temporal lobe, and it is involved with object and visual identification and recognition. The V2 and V4 areas belong to it.

In the same hypothesis, the dorsal stream, the *where* pathway, is involved with processing the object's spatial location relative to the viewer and with speech repetition. The V3 and V5 areas belong to it.

![[Pasted image 20220329155953.png|300]]



## Ventral Stream
![[Pasted image 20220406150731.png|300]]
## Temporal Lobe
The temporal lobe subdivides into the superior temporal lobe, the middle temporal lobe, and the inferior temporal lobe. It houses several critical brain structures including the hippocampus and the amygdala.

## Occipital Lobe
The occipital lobe is the visual processing center of the mammalian brain containing most of the anatomical region of the visual cortex (V1,V2,V3,V4,V5).

## Visual Cortex
[reference](https://www.seevividly.com/info/Physiology_of_Vision/The_Brain/Visual_System/Visual_Cortex)
The visual cortex is located in the **occipital lobe** of the brain and is primarily responsible for interpreting and processing visual information received from the eyes. The amount of visual information received and processed by the visual cortex is truly massive. The visual cortex is divided into six critical areas depending on the structure and function of the area. These are often referred to as **V1, V2, V3, V4, V5**, and the **inferotemporal cortex (IT)**. The primary virtual cortex (V1) is the first stop for visual information in the occipital lobe.

### V1
V1 can be thought of as a sorting area. Cells from the entire visual field are represented, but the majority of V1 is dedicated to cells associated with the foveal (central) vision. The fovea is the small anatomical area of the retina responsible for fine-detail vision.

The main function is to process all the incoming visual information and pass the correct information to the more specialized areas of the cortex: The more specialized areas are termed the **extrastriate cortex**, and include visual areas **V2, V3, V4, V5,** and the **inferotemporal cortex**.

### V2
V2 receives information directly from V1 and passes information to V3, V4, and V5. There is also a feedback loop to send signals back to V1.

### V3
V3 communicated directly with the respective dorsal and ventral subsystems of V2. Dorsal V3 seems to play a role in processing motion, while ventral V3 may play a role in color sensitivity. V3 as a whole is less well-defined compared to other areas of the visual cortex.

### V4 - extrastriate cortex
V4 receives information from V2 and is part of the ventral processing stream. Cells in V4 are very responsive to color.

### V5 - middle temporal cortex
V5 is part of the dorsal processing pathway and contains cells highly sensitive to motion.

### IT - inferotemporal cortex
The inferotemporal cortex is located along the lower (inferior) portion of the temporal lobe. This area of the brain is part of the ventral processing stream and seems to respond best so simple shapes (circle, square, etc.).








# Function
## Flow of Information
When viewing an object, information ﬂows from the retina, through the
LGN, to V1, then onward to V2, then V4, then IT. This happens within the ﬁrst 100ms of glimpsing an object. 

If a person is allowed to continue looking at the object for more time, then information will begin to ﬂow backwards as the brain uses top-down feedback to update the activations in the lower level brain areas. 

However, if we interrupt the person’s gaze, and observe only the ﬁring rates that
result from the ﬁrst 100ms of mostly feedforward activation, then IT proves to be
very similar to a convolutional network. Convolutional networks can predict IT
ﬁring rates, and also perform very similarly to (time limited) humans on object
recognition tasks.

## Retina and Optic Nerve
The neurons in the retina perform some simple preprocessing of the image but do not substantially alter the way it is represented. The image then passes through the optic nerve 


## Lateral Geniculate Nucleus
After the optica nerve, the signal passes the brain region called the lateral geniculate nucleus.


## V1
The main role, as far as we are concerned
here, of both of these anatomical regions is primarily just to carry the signal from
the eye to V1, which is located at the back of the head.
V1 is the ﬁrst area of the brain that begins to
perform signiﬁcantly advanced processing of visual input
![[Pasted image 20220328155730.png|400]]

**A convolutional network layer is designed to capture three properties of V1:**

1. V1 is arranged in a spatial map. It actually has a two-dimensional structure
mirroring the structure of the image in the retina.

2. V1 contains many **simple cells**. A simple cell’s activity can to some extent
be characterized by a linear function of the image in a small, spatially
localized receptive ﬁeld. The detector units of a convolutional network are
designed to emulate these properties of simple cells.

3. V1 also contains many **complex cells**. These cells respond to features that
are similar to those detected by simple cells, but complex cells are invariant
to small shifts in the position of the feature. This inspires the pooling units
of convolutional networks. Complex cells are also invariant to some changes
in lighting that cannot be captured simply by pooling over spatial locations.
These invariances have inspired some of the cross-channel pooling strategies
in convolutional networks, such as maxout units.

**Note:** Activation functions usually have a region where they behave like a linear regime. Maybe the simple and complex cells are the same type of function, but are have different *weights* and, therefore, enjoy different parts of the same function.

**Though we know the most about V1, it is generally believed that the same
basic principles apply to other areas of the visual system.**

The V1 area is not known for handling complex transformations, like the IT.

### Gabor functions
Reverse correlation shows us that most V1 cells have **weights** that are described
by **Gabor functions**. 
The Gabor function describes the weight at a 2-D point in the image. We can think of an image as being a function of 2-D coordinates, $I(x, y)$. The response of a cell can be given by applying the weights $w(x,y)$:
$$\begin{equation}
s(I) = \sum_{x,y} w(x,y) I(x,y)
\end{equation}$$
Speciﬁcally, $w(x, y)$, takes the form of a Gabor function:
$$\begin{equation}
w(x,y,\alpha,\beta_x,\beta_y,f,\phi,x_0,y_0,\tau) = \alpha \exp(-\beta_x x'^{2}-\beta_y y'^{2})\cos(f x'+\phi)
\end{equation}$$
where
$$\begin{equation}
x' = (x-x_0)\cos(\tau) + (y-y_0)\sin(\tau)
\end{equation}$$
and
$$\begin{equation}
y' = -(x-x_0)\sin(\tau) + (y-y_0)\cos(\tau)
\end{equation}$$
Here, $\alpha$, $\beta_x$, $\beta_y$, $f$, $\phi$, $x_0$, $y_0$, and $\tau$ are parameters that control properties of the Gabor function.

The following figure shows some examples of Gabor functions with diﬀerent settings of these parameters.
![[Pasted image 20220328235829.png|500]]


Some of the most striking correspondences between neuroscience and machine
learning come from visually comparing the features learned by machine learning
models with those employed by V1. Most deep learning algorithms learn Gabor-like functions when applied to natural images in their first layer.


## Extrastriate Areas (V2,3,4, and 5) 
midlevel ventral areas—such as V4, the dominant cortical input to IT—exhibit intermediate levels of object selectivity andvariation tolerance.

## Inferior Temporal Cortex (IT)
The closest analog to a convolutional network’s last layer of features is a brain area called the inferotemporal cortex (IT).

The part that is responsible for biological vision is the Inferior Temporal (IT) Cortex, which is **the cerebral cortex on the inferior convexity of the temporal lobe in primates including humans**

![[Pasted image 20220327164232.png|300]]





If we interrupt the person’s gaze, and observe only the ﬁring rates that result from the ﬁrst 100ms of mostly feedforward activation, then IT proves to be very similar to a convolutional network.

#### [Fast Readout of Object Identity from Macaque Inferior Temporal Cortex](http://cbcl.mit.edu/publications/ps/hung_kreiman_science_2005.pdf)
This paper read too the firing of some neurons of the IT brain region of monkeys after showing them some images and then trained a linear classifier to use those firings to predict the image. It found out these firings were strongly invariant to certain transformations like rotations and scaling. This does not necessarily mean the transformation of the input into invariant representations happens in the IT region, it can happen before.



## Differences between Biological and Computer Vision
* The human eye is mostly very low resolution, except for a tiny patch called the
fovea. The fovea only observes an area about the size of a thumbnail held at
arms length. Though we feel as if we can see an entire scene in high resolution,
this is an illusion created by the subconscious part of our brain, as it stitches
together several glimpses of small areas. Most convolutional networks actually
receive large full resolution photographs as input. The human brain makes several eye movements called saccades to glimpse the most visually salient for task-relevant parts of a scene. Incorporating similar attention mechanisms into deep learning models is an active research direction.

* Even simple brain areas like V1 are heavily impacted by feedback from higher
levels. Feedback has been explored extensively in neural network models but
has not yet been shown to oﬀer a compelling improvement.

* While feedforward IT ﬁring rates capture much of the same information as
convolutional network features, it is not clear how similar the intermediate
computations are. The brain probably uses very diﬀerent activation and
pooling functions. An individual neuron’s activation probably is not well-
characterized by a single linear ﬁlter response. A recent model of V1 involves
multiple quadratic ﬁlters for each neuron. Indeed our
cartoon picture of “simple cells” and “complex cells” might create a non-
existent distinction; simple cells and complex cells might both be the same
kind of cell but with their “parameters” enabling a continuum of behaviors
ranging from what we call “simple” to what we call “complex.”

* It is also worth mentioning that neuroscience has told us relatively little
about how to train convolutional networks.

Some of the most striking correspondences between neuroscience and machine
learning come from visually comparing the features learned by machine learning
models with those employed by V1. Since then, we have found that an extremely wide variety of statistical learning algorithms learn features with Gabor-like functions when applied to natural images. **This includes most deep learning algorithms, which learn these features in their ﬁrst layer.** Because so many diﬀerent learning algorithms learn edgedetectors, it is diﬃcult to conclude that any speciﬁc learning algorithm is the “right” model of the brain just based on the features that it learns (though it can certainly be a bad sign if an algorithm does not learn some sort of edge detector when applied to natural images).
These features are an important part of the statistical structure of natural images.

![[Pasted image 20220328214523.png]]


# Experimental Studies
One way to understand the neuronal activity regarding images is to put an electrode in the neuron itself, display several samples of white noise images in front of the animal’s retina, and record how each of these samples causes the neuron to activate. We can then ﬁt a linear model to these responses in order to obtain an approximation of the neuron’s weights. This approach is known as **reverse correlation**.


# Random Statements
* Core object discrimination is defined as the ability to discriminate between two or more objects in visual images presented under high view uncertainty in the central visual field (10°) for durations that approximate the typical primate free-viewing fixation duration (200 ms).