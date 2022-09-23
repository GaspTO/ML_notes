# Overview
This paper tries to understand the behaviour of networks versus primates. The idea is to understand whether the networks can classify the pictures with the same accuracy as primates and see if, when it has trouble, it matches the trouble of primates.


# Data 
the data is publicly available on brian-score.

# Method
The structure of the methods is divided into explaining how they collected the data from monkeys, humans and models. Then they talk about the specific metrics they use to analyse the data.

They show both humans and monkeys images from a collection of 24 objects, with 10 images each, and ask them to choosen between two objects, the correct one they saw and the wrong one (that throughout this article is called *distractor*).

The way they apply this performance to neural networks, is by passing the seen imagen through it and then zeroing the probabilities of the objects not chosen, normalizing the probabilities of the correct and disctractor objects.

They use 4 metrics that can be summarized in the following table.
![[Pasted image 20220426150644.png]]
In simpler words:
1. **One-versus-all-object (BO1)** is the how easy it is to descrimate that object in comparison to all others.
2. **One-versus-other-object (BO2)** is how easy it is to descrimate an object when compared to a given distractor.
3. **One-versus-all-image (BI1)** is how easy it is to descrimate the image as the correct object.
4. **One-versus-other-image (BI2)** is how easy it is to discriminate the correctly the image when compared to a given distractor.



To compare between two models, they use a concept called behavioural consistency. Read the article to know what that is (it's looks like a normalized RSA ).

# Results
Warm colors indicate lower discriminability.
## Object Discrimination
### B.O1
![[Pasted image 20220426153409.png|300]]

![[Pasted image 20220426153417.png|250]]
The correlation in BO1 to the human behavioural signature is quite good. Indeed, we could not reject the hypothesis that DCNN IC models are primate like.



### B.O2
![[Pasted image 20220426153432.png|700]]

![[Pasted image 20220426153441.png|300]]
We observed strong qualitative similarities between the pairwise object confusion patterns of all of the high level visual systems (e.g., camel and dog are often confused with each other by all three systems). This similarity is quantified in Figure 2E, which shows the human consistency of all examined visual system models with respect to this metric. Similar to the B.O1 metric, we observed that both a pool of macaque monkeys and a held-out pool of humans are highly human consistent with respect to this metric.


## Imagine Discrimination
## BI1
![[Pasted image 20220426161447.png]]

![[Pasted image 20220426161518.png|250]]
ows the human consistency
with respect to the B.I1n signature for all tested models. Unlike with object-level behavioral metrics, we now observe a divergence between DCNN IC models and primates. Both the monkey pool and the held-out human pool remain highly human consistent, but all DCNN models were significantly less human consistent and well outside of the defined “primate zone” of B.I1n human consistency. Indeed, the hypothesis that the human consistency of DCNN IC models is within the primate zone is strongly rejected.


## BI2
![[Pasted image 20220426161550.png]]

![[Pasted image 20220426161601.png|250]]

When we look at i