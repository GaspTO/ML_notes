# References
[1] Pattern Recognition and Machine Learning, Chapter 1 (Mostly 1.2.3, 1.2.5).
[2]  [Bayesian Probability](https://en.wikipedia.org/wiki/Bayesian_probability)
[3]  [Bayesian Inference](https://en.wikipedia.org/wiki/Bayesian_inference)

# Overview
According to [2], *"in the Bayesian view, a probability is assigned to a hypothesis, whereas under [frequentist inference](https://en.wikipedia.org/wiki/Frequentist_inference "Frequentist inference"), a hypothesis is typically tested without being assigned a probability."*

Also, According to [2]:
-  The use of random variables, or more generally unknown quantities, to model all sources of uncertainty in statistical models including uncertainty resulting from lack of information
-  The need to determine the prior probability distribution taking into account the available (prior) information.
-  The sequential use of Bayes' theorem: as more data become available, calculate the posterior distribution using Bayes' theorem; subsequently, the posterior distribution becomes the next prior.
* the frequentist probability of a hypothesis is either 0 or 1. In Bayesian statistics, the probability that can be assigned to a hypothesis can also be in a range from 0 to 1 if the truth value is uncertain.

# Data inference
Let's see how a frequentist and a bayersian approach the problem of using data, in the form of $\mathbf{(x,t)}$, to predict a new point $\tilde{x}$. We use a gaussian distribution to predict the uncertainty about the prediction:
![[Pasted image 20220829203801.png|400]]
In formal terms:
![[Pasted image 20220829200611.png]]
## Frequentists
Before inference, the frequentists will look for a specific parameter $\mathbf{w}$ that explains the data the best. To do that, we can use the negative log likelihood.
![[Pasted image 20220829200920.png]]
and we can maximize this expression on the basis of $\mathbf{w}$, which matters only the first term, called squared error.

## Bayesian
The bayesian approach assumes a prior believe around the parameters $\mathbf{w}$, which in this case, as an example, we will consider the prior to also be a gaussian
![[Pasted image 20220829201135.png]]
Then, we can use the bayes rule and get the posterior distribution,
![[Pasted image 20220829201201.png]]

Then, to do inference a new point, we use the posterior, we just calculated, as a prior
 ![[Pasted image 20220829205017.png|450]]

 For gaussian models applied to curve fitting, it is possible to close the previous two expressions. See the book for more.

###  Maximum Posterior
We can apply the negative log likelihood to the likelihood function with the prior, like a frequentist would
![[Pasted image 20220829201201.png]]
we get a very similar expression as before, except with a new term that is a L2 norm,
![[Pasted image 20220829201229.png]]

This technique is called **maximum posterior**, or simply **MAP**

From the eyes of a Bayesian, the frequentist is maximizing the posterior
![[Pasted image 20220829213940.png]]

but they are using $p(\mathbf{w}|\alpha)$ independent from $\mathbf{w}$ , so that when the logarithm is applied and then derived, the prior disappears. In order words, the pure frequentist approach believes that the probability of each parameter $\mathbf{w}$.

If you think abut this from the prespective of an **L2** norm, the purpose of $\mathbf{w}^\intercal \mathbf{w}$ is to push the parameters to zero as much as possible, which makes sense with the gaussian prior we applied, which is centered around zero.

