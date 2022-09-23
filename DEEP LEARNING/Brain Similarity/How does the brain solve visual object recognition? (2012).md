# Overview
This paper is quite complete.  Here they review evidence ranging from individual neurons, to neuronal populations, to behavior, to computational models. This is a definitely *must-read* paper regarding this area.

It was written some time ago, but the ideas seem to be applicable to the current state-of-the-art research.

## Problem
core object recognition, *the ability to rapidly recognize objects despite substantial appearance variation*, is solved in the brain via a a cascade of reglexive, largely feedforward computations that culminate in a poweful neuronal representation in the inferior temporal cortex. However the algorithm that produces this solution remains little-understood.

## Quick summary of ideas
The summary of the visual cortex is that the crux of the problem is the invariance and the objective is to *untangle* (see paper to understand this concept) the coding in the pixel space to some other more easily separable. The neurons that seem to be tolerant (not invariant) to some identity-preserving transformations can be found in the IT region of the brain. Another interesting thing is that the iteration between OR-like and AND-like operations can produce patterns of representations that approximate IT neruonal responses.




# Invariance
**The ability to identify objects over a large range of viewing conditions. This so called ‘‘invariance problem’’ is the computational crux of recognition - it is the major stumbling block for computer vision recognition systems.**

In the real world, each encounter with an object is almost entirely unique, because of *identity-preserving image transformations* (scale, pose, position, illumination, clutter...; besides some object are naturally deformable in shape, like bodies and faces...). 

The brain, not only is invariant to many transformations, but it seems to solve this problem rather quickly. Such invariance not only is a hallmark of primate vision, but also is found in evolutionarily less advanced species like rodents.

# Graphical Intuiton
We can see this mechanism as the ventral visual system gradually untangling the coded information received by the retina. Once untangled, it is easier to distinguish which object is which (see next figure).
![[Pasted image 20220406142936.png|500]]
When an object undergoes an identity-preserving transformation, such as a shift in position or a change in pose, it produces a different pattern of population activity, which corre-sponds to a different response vector. Together, the
response vectors corresponding to all possible identity-preserving transformations (e.g., changes in position, scale, pose, etc.) define a low-dimensional surface in this high-dimensional space—an object identity manifold. Different objects will thefore be ‘‘tangled’’ together in the beggining of the ventral pathway.

At higher stages of visual processing, neurons tend to maintain their selectivity for objects across changes in view; this translates to manifolds that are more flat
and separated (more ‘‘untangled’’). object manifolds are thought to be gradually untangled through nonlinear selectivity and invariance computations applied at
each stage of the ventral pathway. In this geometrical perspective, this
amounts to positioning a decision boundary, such as a hyperplane, to separate the manifold corresponding to one object from all other object manifolds. And thus it becomes clear why the representation at early stages of visual processing is problematic for object recognition: a hyperplane is completely insufficient for
separating one manifold from the others because it is highly tangled with the other manifolds.


# Object Representation
**What Do We Know about the Brain’s *Object* Representation?**

## Intro
Briefly, long-term lesion studies, temporary activation/inactivation studies, and neuro-physiological studies (described below) all point to the central role of the ventral visual stream in invariant object recognition. Lesions or inactivation of anterior regions,especially the inferior temporal cortex (IT), can produce selective deficits in the ability to distinguish among complex objects. a comparison of monkey IT and human ‘‘IT’’ (LOC) shows strong commonality in the population representation of object categories.

## Hierarchical Areas
The ventral visual stream has been parsed into distinct visual ‘‘areas’’ based on anatomical connectivity patterns, distinctive anatomical structure, and retinotopic mapping. 

Complete retinotopic maps have been revealed for most of the visual field (at least 40 degrees eccentricity from the fovea) for areas V1, V2, and V4 and thus each area can be thought of as conveying a population-based re-representation of each visually presented
image.

Within the IT complex, crude retinotopy exists over the more posterior portion, but it is not reported in the central and anterior regions. Thus, while IT is commonly parsed into subareas such as TEO and TE or posterior IT (pIT), central IT (cIT) and anterior IT (aIT), it is unclear if IT cortex is more than one area, or how the term *area* shuld be applied. One striking illustration of this is recent monkey fMRI work, which shows that there are three to six or more smaller regions within IT that may be involved in face *processing*.

This suggests that, at the level of IT, behavioral goals (e.g., object categorization) may be a better spatial organizing principle tha retinotopic maps.

### Retinotopy
This part is not in the paper, they just assume everyone knows what retinotopy is, so I'll write it here.
*Retinotopy* is the mapping of visual input from the retina to neurons, particularly those in the visual steam. Retinotopic maps in cortical areas other than V1 are typically more complex, in the sense that adjacent points of the visual field are not always represented in adjacent regions of the same area. 
Retinotopy mapping is done with fMRIs. The subject inside the fMRI machines focuses on a point, then the retina is stimulated with a circular image or angled lines about focus point.

## Cortical Code
The only known means of rapidly conveying information through the ventral pathway is via the spiking activity that travels along axonswhile all spike-timing codes cannot easily (if ever) be ruled out, rate codes over $\sim$ 50 ms.
intervals are not only xasy to decode by downstream neurons, but appear to be sufficient to support recognition behavior. Thus, we consider the neuronal representation in a given
cortical area (e.g., the ‘‘IT representation’’) to be the spatio-temporal pattern of spikes produced by the set of pyramidal neurons that project out of that area (e.g., the spiking patterns traveling along the population of axons that project out of IT).

## IT Population Appears Sufficient
the ventral stream produces an IT pattern of activity that can directly support robust, real-time visual object categorization and identification, even in the face of changes in object
position and scale, limited clutter, and changes in background context.
Specifically, simple weighted summations of IT spike counts over short time intervals lead to high rates of cross-validated performance for randomly selected populations of only a few hundred neurons. IT neuronal populations are demonstrably better at object identification and categorization than populationsat earlier stages of the ventral pathway.

## Summary
1.  spike counts in $\sim$ 50 ms IT decoding windows convey information about visual object identity. 
2.  This information is available in the IT population beginning $\sim$ 100 ms after image presentation (see Figure 4A). 
3. The IT neuronal representation of a given object across changesin position, scale, and presence of limited clutter is untangled.
4. These codes are readily observed in passively viewing subjects, and for objects that have not been explicitly trained

In sum, our view is that the ‘‘output’’ of the ventral stream is reflexively expressed in neuronal firing rates across a short interval of time $\sim$ 50 ms) and is an ‘‘explicit’’ object representation (i.e., object identity is easily decodable), and the rapid production of this
representation is consistent with a largely feedforward, nonlinear processing of the visual input. 


# IT Single Neurons
**How do these IT neuronal population phenomena (above) depend on the responses of individual IT neurons?**
Most neurophysiological studies agree that most IT neurons respond to many different visual stimuli and, thus, cannot be narrowly tuned ‘detectors’ for particular complex objects.  Moreover, IT neurons with the highest shape selectivities are the least tolerant to changes in position, scale, contrast, and presence of visual clutter a finding inconsistent with *gnostic units* or *grandmother cells*, biut one that arises naturally from feeddforward computational models. That is, single IT neurons do not appear to act as sparsely active, invariant detectors of specific objects, but, rather, as elements of a population that, as a whole, supports object recognition. This implies that individual neurons do not need to be
invariant

Such findings argue for a distributed representation of visual objects in IT, as suggested previously. Instead, the key single-unit property is called neuronal ‘‘tolerance’’: the ability of each IT neuron to maintain its preferences among objects, even if only over a limited transformation range.

Mathematically, tolerance amounts to separable single-unit response surfaces for object shape and other object variables such as position and size


# Algorithm
![[Pasted image 20220406171857.png]]

## Solution Untaggling
the object manifolds conveyed to primary visual cortical area V1 are nearly as tangled as the pixel representation. As V1 takes up the task, the number of output neurons, and hence the total dimensionality of the V1 representation, increases approximately 30-fold (see previous pfigure). Because V1 neuronal responses are nonlinear with respect to their inputs (from the LGN), this dimensionality expansion results in an overcomplete population rerepresentation in which the object manifolds are more ‘‘spread out.’’ Indeed, simulations show that a V1-like representation is clearly better than retinal-ganglion-cell-like (or pixel-based) representation, but still far below human performance for real-world recognition problems.

We should look at the brain in layers. The next figure summarizes some ideas about the different layers:
![[Pasted image 20220407112829.png]]

## Global-scale
What happens after V1? (V2, V4, pIT, aIT)?
1. One framework postulates that each successive visual area serially adds more processing power so as to solve increasingly complex tasks, such as the untangling of object identity manifolds. (It's like a serial assembly line).
2. Another postulates the additional idea that the ventral stream hierarchy, and interactions between different levels of the hierarchy, embed important processing principles analogous to those in large hierarchical organizations. The higher levels communicate with the lower ones (like the orders of a lieutenant to a soldier).

A central issue that separates the largely feedforward ‘‘serial-chain’’ framework and the feedforward/feedback ‘‘organized hierarchy’’ framework is whether re-entrant areal communication (e.g., spikes sent from V1 to IT to V1) is necessary for building explicit object representation in IT.  It is likely that a compromise view is correct. For example, the visual system can be put in noisy or ambiguous conditions (e.g., binocular rivalry) in which
coherent object percepts modulate on significantly slower time scales. The feedback might be used in those circumnstances.

## Meso-scale
One key idea implicit in both algorithmic frameworks is the idea of abstraction layers each level of the hierarchy need only be concerned with the ‘‘language’’ of its input area and its local job.

## Local-scale
We and others advocate the additional possibility that each ventral stream subpopulation has an identical meta job description. That way the genetic code could have replicated itself over multiple units, making it simpler and more probable, from an evoluutionary point of view. To support this argument, there has been many who have emphasized the remarkeable architectureal homogeneity of the mammalian neocortex. With some exceptions, each piece of neocortex copies many details of local structure (number of layers and cell types in each layer), interanal connectivity (major connection statistics within that local circuit), and external connectivity (e.g.., inputs from the lower cortical area arrive in layer 4, outputs to the next higher cortical area depart from layer 2/3).

For core object recognition, we speculate that the canonical meta job description of each local cortical subpopulation is to solve a microcosm of the general untangling problem. That is, instead of working on a $\sim$ 1 million dimensional input basis, each cortical subpopulation works on a much lower dimensional input basis (1,000–10,000), which leads to significant advantages in both wiring packing and learnability from finite visual experience.


## Bottom-Up
The previous subtopics talked about a prespective top-down, but a big part of research used to be worried about trying to understand how neurons and circuits formed by them create these whole areas. 

An example of this is the Hubel and Wiesel conceptual model that postulates the existence of two operations in V1 that produce the response propoerties of the *simple* and *complex* cells. Simple cells would implement **AND**-like operations on LGM, producing selectivity (orientation-tuned response) . Complex cells would implement **OR**-like operations, that would be invariante to some properties. 

![[Pasted image 20220407155550.png|400]]

Currently, these models have beem formalized into the ***linear-nonlinear*** (LN) class of encoding models, in which each neuron adds and subtract its inputs, followed by a static nonlinearity to produce a firing rate response. 

LN-style models are far from a synaptic-level model of a cortical circuit, but they can account for a substantial amount of single-neuron response patterns in early visual and auditory cortical areas. Indeed, a nearly complete accounting of early level neuronal response patterns can be achieved with extensions to the simple LN model framework—most notably, by divisive normalization schemes in which the output of each LN neuron
is normalized (e.g., divided) by a weighted sum of a pool of nearby neurons (we refer to here as the ‘‘normalized LN’’ model class, NLN).

We do not know whether the NLN class of encoding models can describe the local transfer function of any output neuron at any cortical locus. The parsimonious view is to assume that the NLN model class is sufficient but that the particular NLN model parameters (i.e., the filter weights, the normalization pool, and the specific static nonlinearity) of each neuron are uniquely elaborated.

## Canonical Cortical Algorithms
We postulate the existence of the following three key conceptual mechanisms:
1. [ARCHITECTURE] Each subpopulation sets up architectural nonlinearities that naturally tend to flatten object manifolds. Specifically, even with random (nonlearned) filter weights, NLN-like models tend to produce easier-to-decode object identity manifolds largely on the strength of the normalization operation similar in spirit to the overcomplete approach of V1.
2. [WEIGHT LEARNING] Each subpopulation embeds mechanisms that tune the synaptic weights to concentrate its dynamic response range to span regions of its input space where images are typically found (e.g., do not bother encoding things you never see). This is the basis of natural image statistics and compression and its importance is supported by the observation that higher levels of the ventral stream are more tuned to natural feature conjunctions than lower levels.
3. [ALGORITHM for INTOLERANCE] Each subpopulation uses an unsupervised algorithm to tune its parameters such that input patterns that occur close together in time tend to lead to similar output responses

# Other
## Facts
1. Half of the non-human primate neocortex is devoted to visual processing (which speaks to the computational complexity of object recognition).
2. Monkeys can perform object recognition in a short time of 250ms and humans 350ms and images can be presented sequentially at rates less than around 100 ms per image.
3. 
4.  





