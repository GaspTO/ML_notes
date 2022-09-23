Paper: [***AutoAugment: Learning Augmentation Policies from Data***](https://arxiv.org/pdf/1805.09501v3.pdf)


# Overview
They learn use Reinforcement Learning to teach an RNN controller to choose a strategy (a set of data transformations in sequence) to apply to data. During training, for each strategy, they train a smaller network and the accuracy of that network is used to update the RNN controller using policy gradient methods.

![[Pasted image 20220315174030.png|400]]


## Search Space
In their search space, a policy consists of 5 sub-policies with each sub-policy consisting of two image operations to be applied in sequence. Additionally, each operation is also associated with two hyperparameters: 
1) the **Probability** of applying the operation, and 
2) the **Magnitude** of the operation.

Example of a policy with 5 sub-policies:
![[Pasted image 20220315174356.png]]

The operations used are fixed and are those from the PIL library and two others: Cutout and SampledPairing. **16 in total:** ShearX/Y, TranslateX/Y, Rotate, AutoContrast, Invert, Equalize, Solarize, Posterize, Contrast, Color, Brightness, Sharpness, Cutout, Sample Pairing.

The **Probability** and **Magnitude** are discriticized into 10 and 11 uniform spaced values, respectively. 

The total search space is $(16 \times 10 \times 11)^2$.

Our goal, however, is to find 5 such sub-policies concurrently in order to increase diversity. The search space with 5 sub-policies then has roughly $(16 \times 10 \times 11)^{10}$.


## Search Algorithm
It has two components: 
1) A RNN controller
2) An RL algorithm, which in this paper was e [[PPO|Proximal Policy Optimization algorithm]].

At each step, the controller predicts a decision produced by a softmax; the prediction is then fed into the next step as an embedding.

In total the controller has 30 softmax predictions in order to predict 5 sub-policies, each with 2 operations, and each operation requiring an operation type, magnitude and probability ($5 \times 2 \times 3 = 30$).


## Training the RNN Controller
After a prediction, a *child model* is trained using the predicted sub-policies and the accuracy of that model in a validation set is the reward signal. For each example in the mini-batch, one of the 5 sub-policies is chosen randomly to augment the image.



## RNN Controller Details
* The RNN controller is a one-layer LSTM with $100$ hidden units at each layer and $2 \times 5B$  softmax predictions for the two convolutional cells.
* Each of the $10B$ predictions of the RNN controller is associated with a probability.
* The joint probability of a child network is the product of all probabilities at these $10B$ softmaxes. This joint probability is used to compute the gradient for the controller RNN.
* The gradient is scaled by the validation accuracy of the child network to update the controller RNN such that the controller assigns low probabilities for bad child networks and high probabilities for good child networks.

## Final Notes
* At the end of the search, we concatenate the sub-policies from the best 5 policies into a single policy (with 25 subpolicies). This final policy with 25 sub-policies is used to train the models for each dataset.
* The above search algorithm is one of many possible search algorithms we can use to find the best policies. It might be possible to use a different discrete search algorithm such as genetic programming or even random search to improve the results in this paper.


# Results
-> Not complete

## ImageNet
![[Pasted image 20220315185123.png|400]]

![[Pasted image 20220315185232.png|400]]



# Criticism
The paper [RandAugment](RandAugment: Practical automated data augmentation with a reduced search space) criticized and showed that, just because some parameters work for some small network, it does not mean it scales when the network is changed or increased.