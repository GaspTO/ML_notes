***This is a general page about self-supervised learning***

There's three types of self-supervised instance/contrastive learning, clustering and Handcrafted pretext tasks.


# Useful Resources
[Talk with Yann Lecun about self-supervised learning (1:36:12)](https://www.youtube.com/watch?v=8L10w1KoOU8&ab_channel=AlfredoCanziani)
[A Survey On Contrastive Self-supervised Learning](https://arxiv.org/pdf/2011.00362.pdf)
[Unsupervised Learning of Visual Features by Contrasting Cluster Assignments](https://arxiv.org/pdf/2006.09882.pdf)


# NLP vs Computer Vision
Self-supervised learning has had a particularly profound impact on NLP. These models are pretrained in a self-supervised phase and then fine-tuned for a particular task, such as classifying the topic of a text.  In the self-supervised pretraining phase, the system is shown a short text in which some of the words have been masked or replaced. The system is trained to predict the words that were masked or replaced.

Predicting missing parts of the input is one of the more standard tasks for SSL pretraining.

These techniques, however, can’t be easily extended to new domains, such as CV. Despite promising early results, SSL has not yet brought about the same improvements in computer vision that we have seen in NLP. The main reason is that it is considerably more difficult to represent uncertainty in the prediction for images than it is for words. When the missing word cannot be predicted exactly (is it “lion” or “cheetah”?), the system can associate a score or a probability to all possible words in the vocabulary: high score for “lion,” “cheetah,” and a few other predators, and low scores for all other words in the vocabulary.

# A Unified View of Self-Supervised Methods
There is a way to think about SSL within the unified framework of an energy-based model (EBM). An EBM is a trainable system that, given two inputs, x and y, tells us how incompatible they are with each other. To indicate the incompatibility between x and y, the machine produces a single number, called an energy. If the energy is low, x and y are deemed compatible; if it is high, they are deemed incompatible:
![[Pasted image 20220127205926.png|300]]
Training an EBM consists of two parts: (1) showing it examples of x and y that are compatible and training it to produce a low energy, and (2) finding a way to ensure that for a particular x, the y values that are incompatible with x produce a higher energy than the y values that are compatible with x. Part one is simple, but part two is where the difficulty lies.

## Joint embedding, Siamese networks
A particular well-suited deep learning architecture to do so is the so-called Siamese networks or joint embedding architecture.	
![[Pasted image 20220127210227.png|250]]
The difficulty is to make sure that the networks produce high energy, i.e. different embedding vectors, when x and y are different images. Without a specific way to do so, the two networks could happily ignore their inputs and always produce identical output embeddings. This phenomenon is called a collapse. When a collapse occurs, the energy is not higher for nonmatching x and y than it is for matching x and y.

There are two categories of techniques to avoid collapse: **contrastive** methods and **regularization** methods.

## Contrastive energy-based SSL
Contrastive methods are based on the simple idea of constructing pairs of x and y that are not compatible, and adjusting the parameters of the model so that the corresponding output energy is large.
![[constrative.gif]]

The method used to train NLP systems by masking or substituting some input words belongs to the category of contrastive methods. But they don’t use the joint embedding architecture. Instead, they use a predictive architecture in which the model directly produces a prediction for y.

But contrastive methods have a major issue: They are very inefficient to train. In high-dimensional spaces such as images, there are many ways one image can be different from another. Finding a set of contrastive images that cover all the ways they can differ from a given image is a nearly impossible task.

What if it were possible to make sure the energy of incompatible pairs is higher than that of compatible pairs without explicitly pushing up on the energy of many incompatible pairs?


## Non-contrastive energy-based SSL
Non-contrastive methods applied to joint embedding architectures is possibly the hottest topic in SSL for vision at the moment. The domain is still largely unexplored, but it seems very promising.
They use various tricks, such as computing virtual target embeddings for groups of similar images or making the two joint embedding architectures slightly different through the architecture or the parameter vector. Barlow Twins tries to minimize the redundancy between the individual components of the embedding vectors.
