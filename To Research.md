# TOP IDEAS
See the descripton of this ideas below.
### Game Tree Bootstrap on latent space (Muzero)

### Are shape-biased models better for transfer learning?

### KNN to evaluate shape-bias

### Benchmark models on shape-bias but using features of middle layers

# General
### Similarity of classifier weight vectors accross models
Wouldn't it be interesting to look at the similarity between the classifier weight vectors of different classes for different networks? For example, if in resnet-50 class A and B have linear independent vectors, does the same happen in resnet-18? What about two completely different resnet-50s?


### Apply loss at rounded probabilities
If we see the imagenet classifier vectors they are not all linear independent, which makes sense since some classes are obviously more correlated with others. When we maximize the label, we are forcing the vectors to be linear independent, which of course fails, but maybe if we didn't... if we applied the loss to the round of the correct class' probabilty - in order words, only update the network if the rounded class probability was not 1, maybe something interesting would happen? maybe it would be faster?

### Batch size scheduling?
Have a batch size scheduler like a learning rate scheduler. Maybe some triangular batch size can help.

### Batch size and Learning rate
Is there a function that relates both? for success?

### Fully connected network with residual connections?
MLPs dont work well because of depth, but with residual connections might?

### Stochastic depth with self-distillation
If residual connections learn an ensemble of functions, couldn't we benefit from teaching some of the functions in that ensemble, what some of the others have learned?

### SSL in encoded video frames
In NLP, the self-supervised approach is to give frases, blank certain words and predict them. In video frames, that's complicated because of the multi-dimensional space of video frames. However, what if, instead of predicting the whole frame, predicting an encoded, compressed, form of the video frame. It can be as simple as some pooling operations, that reduce the frame dimension, to some encoding network.

### Replacing ReLU units for more elementary operations
##### How many neurons you need to learn an arithmetic unit? I was wondering whether putting arithmetic and logic units might be benefitial - but only if they are tough to learn with gradient descent.
Big networks probably learn in parts of them in the middle operations like multiplication, couldn't we replace those subnets for the actual operation?

### Use more than one type of activating function in a network
Does it take the same amount of parameters to learn, say the maximum function, using a net with ReLU activations as it'd take for a network with sigmoid activations.

### Different activation functions in different layers of CNNs
In the DeepLearning book, there are different areas, V1 captures something similar to the first layers of a CNN, but what if the more specific the granularity, the bigger the advantage of other activation functions

### Swapping layers between train CNNs
The first layer of any CNN tends to learn the same. Could we have two ResNets and then switch the learned kernels? Maybe we could just add them? It is cheaper to train 2 networks with N neurons than to train 1 network with 2N neurons, since the cost is quadratic. Maybe we could even be swapping kernels in different combinations until matching the best performance possible.

### Augmenting CNN by adding new filters
Can't we take a pre-trained Resnet-50 and simply add new filters, train these with backpropagation while keeping the pre-trained ones frozen? Maybe study a comparison between freezing and not freezing the pre-trained ones.

### Randomly Resetting neurons
I wonder, if the network gets stuck in an undesirable accuracy, why not reset a layer of neurons and see if it converges to a better one instead of resetting the whole architecture (starting a new). Maybe we can reset random neurons instead of a whole layer, or a feedforward path.

### Mutual Deep Supervision
The idea is similar to mutual distilation, but for deeper supervision

### Does the prioritize replay experience work for supervised datasets?




# Questions
### Does Residual Jump Connections help with convergence?

### Can we progressively train a CNN layer by layer?
Since, as the paper *Visualizing and Understanding Convolutional Networks* says, the shallower filters learn simpler filters. Maybe only update if the maximum value of the softmax is of a different class, instead of trying to set it to 1.0.

### What happens if we do not allow gradients to propagate back through residual connections?**
The question is: are we making the model wider or is simply the fact that the residual connections change the function to the residue that makes it work so well?

### Does projecting data to higher dimensions increase the likelihood of them being separable? 
This question appears in the context of the V1 area having more output points than the input, which means the V1 is augmenting the number of dimensions that explain each datapoint.


# Reinforcement Learning 
### Game Tree Bootstrap on latent space (Muzero)
Use a minimax type algorithm with Muzero or another model that uses a hidden space-state and bootstrap from it.

### SAT solver with reinforcement learning like deepcube.

### RL algorithm
Have policy, p(s,a), and action-value function, q(s,a). Bootstrap using $q(s,a) = \sum(  [r(s,a') + q(s,a')] * p(s,a') )$.

### RL Search Algorithm probabilities
What if we run a balanced/even tree calculating the policy/probability for each child and then the quality of a branch is the joint probability of its best sub-branch.

### Self-Supervised Learning to stabilize RL networks
For instance, can we initialize a network based on the state space using self-supervised learning and then, freeze those features and train a linear function on top of that to predict a Q-function? Maybe it can stabilize it.




# Self-supervised
### Can self-supervised train a multi-layer perceptron
MLPs tend to get stuck in local minimums. Maybe training them with several goals will help them not get stuck.




# Shape-Texture Bias
### Dropout on weights we believe to have to do with shape or texture
If we figure out which neurons have more to do with shape than texture, a way to confirm it would be by changing them. The way to figure out those neurons could be by the technique used in [[Shape or Texture, understanding discriminative features in cnns (2021)]]


### Cosine similarity shape bias classifiers
If you make the cosine similarity between the weight vectors,w, of the classifiers learned when trying to decode texture, which are more correlated? And to decode shape?
This way, we can understand which classes are correlated by texture and which ones are correlated by shape. If we then see the correlation of a pretrained resnet, maybe we can get insight about whether they are correlating on shape or on texture.

For example, if in a resnet-50 classes A and B are 0.3 correlated, but in the shape classifier they are correlated by 0.7 and 0.1 in texture, it might mean that a lot more shape is being captured than texture.

We might even get the correlation matrices of these 3 (imagenet, shape and texture), convert them to flatten vectors and then correlate them. 

### Train shape-texture classifier 
Instead of training a classifier to train texture and another to train shape, let one be right if it gets the shape correct or the texture. Loss = 1-(correct_texture)-(correct_shape). The goal is to see which one is easier to decode, which should be the one the classifier tries to minimize faster.

### Is it architecture or knowledge?
Measure network A on shape-bias. Then distill its knowledge to architecture B and C... Then measure shape-bias. Is it the same? 

### Are shape-biased models better for transfer learning?
Maybe, cause shape biased seems to be better in out of distribution datasets, but, at the same time, maybe not, because shape biased models seem to have to do with the last layer, which is removed in transfer learning.

### Train a model only on pure texture images and another on pure shape (like sketch or silhouette datasets):
* See which transfers better
* See which one can solve blurred and noisy images better (OOD accuracy)

### Benchmark models on shape-bias but using features of middle layers
Like section 8 of the paper [The Origins and Prevalence of Texture Bias in Convolutional Neural Networks](https://arxiv.org/pdf/1911.09071.pdf)
Where they use the hidden features and train to classify shape and texture of a particular mixed dataset and see if it's possible to classify correctly for both experiments.

### KNN to evaluate shape-bias
See this experiment I did and the future work idea [[Reducing Texture bias by fine-tuning]]

### Does a model being good at shape make it shape biased?
See [[Shape bias correlation with other dataset accuracy]]


### Train on cue-conflict and approximate the vector according to shape in a contrastive way

### Benchmark Error Consistency of multiple models 

