Paper: [***RandAugment: Practical automated data augmentation with a reduced search space***](https://arxiv.org/pdf/1909.13719.pdf)


# Overview
It's chooses randomly tuples of transformation and their magniture. The parameters of those tuple choices are 1) the number of tuples and 2) magnitue (the same for every tuple). Then, it is possible to train the original network (unlike [[AutoAugment]] that uses a proxy task which is a smaller network) and see it's accuracy. A grid search can choose good $N$ and $M$ parameters.  
![[Pasted image 20220315190936.png|400]]


