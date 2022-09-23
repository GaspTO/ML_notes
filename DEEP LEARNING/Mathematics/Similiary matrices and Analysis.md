Ppaer: ***Representational similarity analysis – connecting the branches of systems neuroscience***

# Problem
Let's say we have two models that we want to compare for the same inputs. For instace, we have a bunch of images and we want to know how similar our neural network is to the IT part of the brain, when the input is those images.

One possible way to try to compare them is to (linearly) map the neurons in the neural network to the brain, but this can be very time expensive.

# Solution 
The solution given by this paper is to calculate the **representational dissimilarity matrices** of each model, regarding those input. Then transform those RDMs into vectors and compare their correlation.

In other words, we're calculating the similarity between the similarity of each model.


## RDM
**Representational Dissimilarity Matrices** these are correlation matrices. You take the inputs and measure some results for each input using your model (firing of neurons, output... whatever).  

Then, for all pair combination of inputs, we take the similarity between them. The similarity can be the cosine or euclidian distance.
![[Pasted image 20220412135847.png|400]]

The matrice is simetrical regrding the main diagonal. So, usually, to use them, we take either the superior triangular part of the inferior one.

## Significance
The significance is the idea behind the p-values or, in other words, if the null hypothesis is true, how likely it is that I got this extreme result by chance.

Testing the significance of the correlation between 2 RDM matrices is not straightforward because each element in the matrice results from pairs of inputs and these pairs share input elements with each other. 

To test the significance (Here's a [link](https://towardsdatascience.com/how-to-measure-similarity-between-two-correlation-matrices-ce2ea13d8231)), what we can do is to shuffle the rows and columns of one of the matrices we are comparing and test their similarity. If we do this multiple times, then we get a distribution of similiarities that were consequence of randomization.
![[Pasted image 20220412143451.png|400]]
If our similarity fell in the middle of the distribution, then it is very close to the average random similarity and that means it is probably not a correct similarity. If, on the other hand, our similarity was very unlikely by random shuffling, then it is probably right. 

The significance is simply the percentage the randomized process got ours or bigger similarity.