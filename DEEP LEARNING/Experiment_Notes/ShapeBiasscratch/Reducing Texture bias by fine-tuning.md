This is the project I am trying to do in the VISUM summer school in Porto.

# Overview
There has been a paper that seems to imply that the texture bias comes from the latter layers of networks, specifically, from the last mlp.

There's also other that has shown that training networks using data augmentations that destroy texture turn the network into shape biased.

**Question:** Can I turn a pre-trained model into a shape-biased one simply by fine-tuning its last layer using data augmentations that destroy texture, like Gaussian Noise?


# Reports
## 14, 15 July
#### Gaussian Noise
So far, I've tried a Resnet-18 with Gaussian noise and the networks don't seem to fine tune on this augmentation. It's not even that it becomes shape or texture biases, it's simply that when I train will all augmented images, it can't fine-tune properly.

I tried to reduce the noise from (0,20), (0,2), (0,0.5), (0,0.05), and (0,0.005) and so far nothing is really working. 

João told me the mlp might be too small.  So, I tried with resnet-50 but the result was the same

*The gaussian noise parameters are based on the Albumentations library arguments for Gaussian Noise* 

#### Gray
Next I fine-tuned on imagenet, but turned every image gray, and the results were equally bad.

# Discussion
I think the problem with training the mlp on another dataset (like imagenet with gaussian noise) is that it doesn't generalize. The fine-tuning actually seems like a dataset-overfitting, whcih actually explains why the models become texture bias, which lacks generalization.


# Future directions
I think an interesting way forward would be to use KNN to evaluate the features instead of the MLP.
Using KNNs is straightforward with contrastive self-supervised learning, but maybe it will be able to be use for other supervised models. Maybe we can freeze all the layers and use a mlp to approximate features with the same category like in the paper [Supervised Contrastive Learning](https://arxiv.org/pdf/2004.11362)

