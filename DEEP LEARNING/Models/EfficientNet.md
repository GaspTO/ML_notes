Paper: [***EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks***](https://arxiv.org/pdf/1905.11946.pdf)

# Overview
This paper addresses scaling a model and efficiency by doing so (FLOPS of a model). It talks about three dimensions: depth, width (number of filters) and resolution (image/input size):
![[EfficientNet scaling.png]]

We denote  by $w$, $d$, $r$ the coefficients for scaling network width, depth, and resolution.


![[EfficientNet f4.png|300]]


'we empirically observe that different scaling dimensions are not independent. Intuitively, for higher resolution images, we should increase network depth, such that the larger receptive fields can help capture similar features that include more pixels in bigger images. Correspondingly, we should also increase network width when resolution is higher, in order to capture more fine-grained patterns with more pixels in high resolution images. These intuitions suggest that we need to coordinate and balance different scaling dimensions rather than conventional single-dimension scaling.'

**Paper observations:**
1. Scaling up any dimension of network width, depth, or resolution improves accuracy, but the accuracy gain diminishes for bigger models.
2. In order to pursue better accuracy and efficiency, it is critical to balance all dimensions of network width, depth, and resolution during ConvNet scaling.

# Scaling Principle
This paper proposes a method to scale each one of the three dimensions:
![[EfficientNet eq.png|400]]

$\alpha$, $\beta$ and $\gamma$ are constants. They can be found using a grid search for small models, which is fast, and then used for bigger models.

$\phi$  is a user-specified coefficient that controls how many more resources are available for model scaling.

This paper constraints $\alpha \cdot \beta^2 \cdot \gamma^2 \approx 2$  such that for any new $\phi$, the total FLOPS will approximately increase by $2^\phi$.

# Architecture
They start with a baseline network and then they scale it.

![[EfficientNet Baseline.png]]
	
	
Starting from the baseline EfficientNet-B0, we apply our compound scaling method to scale it up with two steps:
1. we first fix $\phi = 1$, assuming twice more resources available, and do a small grid search of $\alpha$, $\beta$, $\gamma$. In particular, we find the best values for EfficientNet-B0 are $\alpha = 1.2$, $\beta = 1.1$, $\gamma = 1.15$, under constraint of $\alpha \cdot \beta^2 \cdot \gamma^2 \approx 2$
2. we then fix $\alpha$, $\beta$, $\gamma$ as constants and scale up baseline network with different $\phi$ using Equation 3, to obtain EfficientNet-B1 to B7. 

# Experiments
Comparing efficient nets to other popular models:
![[EfficientNet B0-B7.png]]

Comparing different values of each scaling components:
![[EfficientNet Table3.png]]



___
#Vision #ImageClassification #Architecture #ConvolutionalNetwork #Scaling 
